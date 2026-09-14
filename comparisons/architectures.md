# FPGA 架构：首批横向比较

核对日期：2026-09-14。以下是助手依据原文整理的阅读起点；个人精读和复现状态由读者自行记录。本表属于 FPGA 架构主线，比较计算复用、数据流和存储组织；Allo 等 HLS 资料作为架构实现与映射方法的支撑参考。模型类型作为适用条件。VLA 加速器仍处于设计前的研究阶段。

## 解决机制与迁移条件

| 条目 | 原工作的问题 | 机制与存储组织 | 迁移到当前研究前需要验证 |
|---|---|---|---|
| [HG-PIPE](https://arxiv.org/html/2407.17879v1#S4) · `hgpipe-2024` | ViT 全局依赖使纯细粒度流水难以连续工作，粗粒度缓冲又占资源 | K/V 完整张量缓冲结合 Q/残差流；按阶段分配并行度；低比特 LUT 运算。详见 §4.1–4.4 | 全层展开所需权重与缓冲能否驻留？闭环单次响应是否能利用跨输入流水？迭代动作头应在哪一层复用硬件？ |
| [FlightLLM](https://arxiv.org/html/2401.03868v2#S3) · `flightllm-2024` | 压缩后的 LLM 仍受计算利用率、访存与指令体积限制 | 统一矩阵引擎与可配置稀疏 DSP 链；decode 激活留片上，权重访问利用 HBM/DDR；长度自适应编译。见 §3–5 | 我们是否需要稀疏支持？VLM 与动作头矩阵形状怎样影响利用率？固定步数是否允许更简单的调度？“激活留片上”不意味着全部权重留片上。 |
| [Allo](https://arxiv.org/html/2404.04815v1#S6) · `allo-2024` | 优化好的单核不能自动组成高效多核设计 | 算法与计算/存储/通信/类型定制分开；组合调度、传播布局、连接流式模块。见 §3–7 | 我们的舍入、饱和与缩放语义能否精确表示？量化矩阵核与非线性模块组合后是否产生布局冲突或额外缓冲？属于工具方法参考。 |
| [EdgeDecoding](https://github.com/IEEE-AICAS/AICAS2025_GC/blob/main/Track2/1_sec/code/README.zh-CN.md) · `edgedecoding-aicas2025` | 端侧 Qwen 解码的权重复用和带宽利用 | HLS 矩阵核与独立累加器通过 stream 相连；QK/RV 有专门模块；工程提供权重 AXI 接口和 KV cache 设计 | 主矩阵核与注意力核的边界是否适合目标工作负载？目标设计采用静态尺度表时，原动态分组缩放接口需要怎样调整？逐 token KV cache 能否用于按时间步重算的张量，须依计算图判断。 |

## 精度、平台与证据边界

| 条目 | 原文精度与平台条件 | 作者提供的证据及查阅位置 | 本库记录方式 |
|---|---|---|---|
| HG-PIPE | DeiT-tiny 的 W4A4/W3A3；ZCU102 和 VCK190。ZCU102 分四部分部署，VCK190 支持完整网络 | 上板吞吐与功耗；流水时序仿真；§5.1–5.5、Table 2 及脚注。DeiT-small 行缺少对应 QAT 准确率 | 平台、分区和位宽分别记录；不能把小模型完整驻留条件推广到大模型。[原文实验](https://arxiv.org/html/2407.17879v1#S5) |
| FlightLLM | OPT-6.7B/LLaMA2-7B；混合 W3/W4/W5，平均 W3.5，A8，并结合稀疏及微调 | U280 实机；VHK158 为经 RTL emulation 校验的周期模拟；§6.1–6.2.1 | 两个平台分开标注；性能收益同时涉及稀疏、量化及存储优化。[原文实验](https://arxiv.org/html/2401.03868v2#S6) |
| Allo | PolyBench 用 FP32；GPT2 355M 示例 W4A8，U280 上板 250 MHz | 单核含综合/布局布线结果；GPT2 上板延迟包含从 kernel launch 开始的主机通信；§8.1–8.3 | 不将 GPT2 的 W4A8 配置套到全部测试；GPU 对照采用 FP16，须保留精度差异。[原文实验](https://arxiv.org/html/2404.04815v1#S8) |
| EdgeDecoding | 定点位宽由导出参数及算子类型共同确定；README 给出 Qwen2.5-0.5B、两种上下文配置 | 竞赛仓库提供 HLS、仿真、Vivado 工程、bitstream/驱动和结果文件；本次只核对文档与源码，未运行复现 | 类型为“工程参考”，本条未核实对应正式论文；README 性能为作者报告。具体器件与 bitstream 对应关系、各算子 W/A 位宽留待工程复现核实。[README](https://github.com/IEEE-AICAS/AICAS2025_GC/blob/main/Track2/1_sec/code/README.zh-CN.md) |

这里的“上板”“模拟”等描述的是作者证据，不表示本库维护者已完成复现。任何性能数字进入后续图表前，至少绑定模型/形状、batch、精度、平台、频率、计时范围和证据类型；缺失项写“待核实”。

## 工程源码定位

- EdgeDecoding 的矩阵核与累加器连接：[GEMM_PERMUTE.cpp](https://github.com/IEEE-AICAS/AICAS2025_GC/blob/main/Track2/1_sec/code/reproduce/hls_design/case/GEMM_PERMUTE.cpp)。注意其中 T、CIP、COP 与分组大小的关系。
- EdgeDecoding 的注意力专用模块：[qk_gemm.h](https://github.com/IEEE-AICAS/AICAS2025_GC/blob/main/Track2/1_sec/code/reproduce/hls_design/src/qk_gemm.h)、[rv_gemm.h](https://github.com/IEEE-AICAS/AICAS2025_GC/blob/main/Track2/1_sec/code/reproduce/hls_design/src/rv_gemm.h)。不能根据共享主矩阵核推断所有乘法都走一个核。
- EdgeDecoding 的位宽入口：[common.h](https://github.com/IEEE-AICAS/AICAS2025_GC/blob/main/Track2/1_sec/code/reproduce/hls_design/src/common.h)。`DW_AQ`/`DW_WQ` 依赖外部参数；累加器和非线性中间值另有位宽。
- EdgeDecoding 的动态分组缩放：[quantizer.h](https://github.com/IEEE-AICAS/AICAS2025_GC/blob/main/Track2/1_sec/code/reproduce/hls_design/src/quantizer.h)。读取当前输入组的绝对最大值，计算移位尺度后量化。
- Allo 的论文版本复现入口：[PLDI 2024 artifact](https://github.com/cornell-zhang/allo-pldi24-artifact)。当前工具仓库后续功能不自动算作 2024 论文贡献。

## 下一次横向总结要回答的一个问题

在同一张工作负载表上，分别画出“整层空间展开”和“算子间局部流水＋矩阵核复用”两种候选组织方式，并标注每条边的数据大小、产生时机、最后一次使用时机。再检查 Allo 等工具能否帮助实现和组合所需模块。先比较必须保存和搬移的数据；不同论文的 images/s、decode tokens/s 和 VLA 策略延迟分别保留其工作负载与计时条件。

从 [HG-PIPE 示例笔记](../notes/hgpipe-2024.md) 中的纸笔练习开始。
