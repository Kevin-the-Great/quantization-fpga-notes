# 量化算法与 FPGA 架构阅读笔记

[English](README.md) | [中文](README.zh-CN.md)

围绕**量化算法**与 **FPGA 架构**两条主线的持续阅读库。当前应用方向是 VLA；方法来源开放到 LLM、ViT、CNN、扩散模型，以及其他具有可迁移计算或存储特征的应用。

量化主线研究低比特表示、误差与校准方法；FPGA 架构主线研究计算、存储和数据流组织，HLS 与映射工具作为实现支撑。收录其他领域的论文时，明确它帮助解决哪条主线中的什么问题。

**按技术问题组织，按会议期刊发现，按适用条件比较。** 每篇收录资料需要说明：解决什么问题、依赖什么条件、对当前研究有什么启发，以及还需要验证什么。

更新日期：2026-09-16。共 **26 篇论文与 1 项工程参考**，其中 **13 篇为 2026 年论文/预印本**。DiTPA（ISCA 2026）现为下一次组会的优先候选。初筛笔记由助理辅助整理；个人阅读、复现与汇报进度分别记录。

下表日期对应已核实的公开版本：通常为 arXiv v1，UDP 为出版社在线日期，DiTPA 为机构记录的出版日期；不保证是最早披露时间。**2026 分组包含该年录用/发表的论文和该年预印本**；LUT-LLM 的预印本始于 2025 年 11 月。“已录用”表示核实了官方议程，“已发表”表示核实了正式出版来源。详见[目录日期字段](data/README.md)。

**下一次组会优先候选：[DiTPA — ISCA 2026](notes/ditpa-2026.md)。** 围绕动作规划中的冗余怎样影响加速架构展开。[准备安排](weekly/2026-09-16.md) · [汇报提纲](talks/ditpa-2026.md)。此前候选仍保留在[阅读安排](weekly/2026-09-14.md)。

[量化算法](#量化算法) · [FPGA 架构](#fpga-架构) · [会议与期刊](venues.md) · [阅读笔记](notes/hgpipe-2024.md) · [周记录](weekly/2026-09-16.md) · [维护方法](WORKFLOW.md)

## 量化算法

比较重构、缩放、旋转与时间步校准，保留原始模型、数值格式和适用条件。完整分析见[量化方法横向比较](comparisons/quantization.md)。

### 2026 年论文

| 公开版本日期 | 场合 / 状态 | 论文 | 核心机制与比较点 |
|---|---|---|---|
| 2026-07-02 | arXiv · 预印本 | [OrbitQuant](https://arxiv.org/abs/2607.02461) | 归一化旋转与共享非均匀码本；免范围校准仍有在线运算。 [笔记](notes/orbitquant-2026.md) |
| 2026-05-27 | arXiv · 预印本 | [HoloQ-VLA](https://arxiv.org/abs/2605.28803) | 复合旋转与逐时间步尺度，用于语言与扩散动作模块的 W4A4；8 月 11 日更新至 v3。 [笔记](notes/holoq-vla-2026.md) |
| 2026-05-03 | FCCM · 已录用 | [ViM-Q](https://arxiv.org/abs/2605.01935) | APoT 权重、逐 token 激活量化和查表/SSM 流水；功耗为估计值。 [笔记](notes/vim-q-2026.md) |
| 2026-04-24 | FCCM · 已录用 | [HGQ-LUT](https://arxiv.org/abs/2604.22293) | 以资源目标训练量化逻辑查表层；硬件证据为 OOC 布局布线。 [笔记](notes/hgq-lut-2026.md) |
| 2026-04-13 | ICML · 已录用 | [ReSpinQuant](https://arxiv.org/abs/2604.11080) | 逐层旋转与低秩残差基底修正；比较精度收益和在线修正代价。 [笔记](notes/respinquant-2026.md) |
| 2026-03-19 | arXiv · 预印本 | [6Bit-Diffusion](https://arxiv.org/abs/2603.18742) | NVFP4/INT8 激活路由结合时间缓存；区分精度选择与跳算收益。 [笔记](notes/6bit-diffusion-2026.md) |
| 2026-02-23 | CVPR · 已发表 | [QuantVLA](https://openaccess.thecvf.com/content/CVPR2026/html/Zhang_QuantVLA_Scale-Calibrated_Post-Training_Quantization_for_Vision-Language-Action_Models_CVPR_2026_paper.html) | 用注意力温度与输出能量校准修正选择性 W4A8；扩散模块的注意力投影保留浮点。 [笔记](notes/quantvla-2026.md) |
| 2026-02-03 | ICLR · 已发表 | [QVLA](https://proceedings.iclr.cc/paper_files/paper/2026/hash/fa064215307efaad75bebc7a2e3194a4-Abstract-Conference.html) | 根据动作敏感性分配通道位宽；权重位宽为平均预算，projector 与动作头保留 BF16。 [笔记](notes/qvla-2026.md) |
| 2026-01-14 | ACL · 已发表 | [MXFP PTQ Benchmark](https://aclanthology.org/2026.acl-long.1854/) | MXFP 下的 PTQ、共享尺度误差及模块敏感性；MXFP4 与 INT4 分开比较。 [笔记](notes/mxfp-ptq-benchmark-2026.md) |
| 2025-11-09 | FCCM · 已录用 | [LUT-LLM](https://arxiv.org/html/2511.06174v2) | 向量联合量化和存储查表；需要模型转换与训练。预印本始于 2025 年。 [笔记](notes/lut-llm-2026.md) |

### 2025 年下半年补充

| 公开版本日期 | 场合 / 状态 | 论文 | 核心机制与比较点 |
|---|---|---|---|
| 2025-11-10 | ICCAD · 已发表 | [QUARK](https://arxiv.org/html/2511.06767v1) | 通过量化和近似共享非线性子算子；比较电路复用与并发执行。 [笔记](notes/quark-2025.md) |
| 2025-09-30 | WASPAA · 已录用 | [Audio DiT PTQ](https://arxiv.org/abs/2510.00313) | 时间步缩放与 FP16 低秩补偿；原文 A4/A8 表格标注存在待核实矛盾。 [笔记](notes/audio-dit-ptq-2025.md) |

### 前序背景

| 年份 | 场合 | 论文 | 核心机制与比较点 |
|---|---|---|---|
| 2025 | ICLR | [SpinQuant](https://proceedings.iclr.cc/paper_files/paper/2025/hash/e5b1c0d4866f72393c522c8a00eed4eb-Abstract-Conference.html) | 学习正交旋转；比较旋转优化成本、可折叠变换和在线变换 |
| 2024 | NeurIPS | [QuaRot](https://proceedings.neurips.cc/paper_files/paper/2024/hash/b5b939436789f76f08b9d0da5e81af7c-Abstract-Conference.html) | 用旋转缓解异常值；区分低比特表示与实际算子精度 |
| 2024 | NeurIPS | [PTQ4DiT](https://proceedings.neurips.cc/paper_files/paper/2024/hash/72d32f4fe0b7af03732bd227bf1c4a5f-Abstract-Conference.html) | DiT 通道显著性平衡与跨时间步校准 |
| 2024 | MLSys | [AWQ](https://proceedings.mlsys.org/paper_files/paper/2024/hash/42a452cbafa9dd64e9ba4aa95cc1ef21-Abstract-Conference.html) | 激活感知的权重量化；关注缩放与实际内核实现 |
| 2023 | ICLR | [GPTQ](https://arxiv.org/abs/2210.17323) | 用二阶信息做权重重构和误差补偿 |
| 2023 | ICML | [SmoothQuant](https://proceedings.mlr.press/v202/xiao23c.html) | 等价通道缩放；比较激活与权重的量化难度、静态与动态尺度 |
| 2023 | ICCV | [Q-Diffusion](https://arxiv.org/abs/2302.04304) | 时间步校准与分支量化；关注迭代误差与结构前提 |

## FPGA 架构

比较计算阵列、数据流、硬件复用、流水线和存储组织；HLS 与映射工具作为实现支撑。完整分析见[FPGA 架构横向比较](comparisons/architectures.md)。

### 2026 年论文

| 公开版本日期 | 场合 / 状态 | 论文 | 核心机制与比较点 |
|---|---|---|---|
| 2026-08-04 | ISCA · 已发表 | [DiTPA](https://doi.org/10.1109/ISCA66397.2026.00188) | 动作预测、去噪复用与多模态调度。架构迁移参考；公开 artifact 为模拟评估，硬件平台待全文核实。[笔记](notes/ditpa-2026.md) |
| 2026-05-03 | FCCM · 已录用 | [ViM-Q](https://arxiv.org/abs/2605.01935) | APoT 权重、逐 token 激活量化和查表/SSM 流水；功耗为估计值。 [笔记](notes/vim-q-2026.md) |
| 2026-04-24 | FCCM · 已录用 | [HGQ-LUT](https://arxiv.org/abs/2604.22293) | 以资源目标训练量化逻辑查表层；硬件证据为 OOC 布局布线。 [笔记](notes/hgq-lut-2026.md) |
| 2026-04-23 | FCCM · 已录用 | [GraphLeap](https://arxiv.org/abs/2604.21290) | 建图与特征更新并行；改变依赖关系后需要微调。 [笔记](notes/graphleap-2026.md) |
| 2026-02-21 | FPGA · 已发表 | [UDP](https://doi.org/10.1145/3748173.3779194) | 参数化 DSP 打包及有符号修正；按实际操作数位宽核算有效乘积。 [笔记](notes/udp-2026.md) |
| 2025-11-09 | FCCM · 已录用 | [LUT-LLM](https://arxiv.org/html/2511.06174v2) | 向量联合量化和存储查表；需要模型转换与训练。预印本始于 2025 年。 [笔记](notes/lut-llm-2026.md) |

### 2025 年下半年补充

| 公开版本日期 | 场合 / 状态 | 论文 | 核心机制与比较点 |
|---|---|---|---|
| 2025-11-10 | ICCAD · 已发表 | [QUARK](https://arxiv.org/html/2511.06767v1) | 通过量化和近似共享非线性子算子；比较电路复用与并发执行。 [笔记](notes/quark-2025.md) |
| 2025-07-04 | ICCAD · 已发表 | [Hummingbird](https://arxiv.org/html/2507.03308v2) | DSP 复用、DDR 对齐与 GQA 缓冲；W4 存储和 INT24 向量计算分别记录。 [笔记](notes/hummingbird-2025.md) |

### 前序背景

| 年份 | 场合 | 论文 | 核心机制与比较点 |
|---|---|---|---|
| 2024 | ICCAD | [HG-PIPE](https://arxiv.org/html/2407.17879v1) | ViT 混合粒度流水线；比较全局依赖、缓冲与并行度。[阅读笔记](notes/hgpipe-2024.md) |
| 2024 | FPGA | [FlightLLM](https://arxiv.org/abs/2401.03868) | LLM 计算与存储映射；比较低比特、稀疏和存储层次的配合 |
| 2024 | PLDI / PACMPL | [Allo](https://www.csl.cornell.edu/~zhiruz/pdfs/allo-pldi2024.pdf) | HLS 支撑参考：可组合的硬件定制与多计算核映射 |

### 工程参考

| 年份 | 来源 | 实现 | 阅读重点 |
|---|---|---|---|
| 2025 | AICAS Grand Challenge · Track 2 | [EdgeDecoding / 1_sec](https://github.com/IEEE-AICAS/AICAS2025_GC/blob/main/Track2/1_sec/code/README.zh-CN.md) | HLS 矩阵核、独立累加器、注意力模块与存储接口；按工程资源记录 |

## 横向比较关注什么

| 主线 | 方法与前提 | 结果需要绑定的条件 |
|---|---|---|
| 量化算法 | 重构目标、校准数据、缩放与旋转、位宽及例外、额外在线运算 | 模型、量化范围、评测协议、任务指标 |
| FPGA 架构 | 计算复用、流水线、片上缓冲、外存通信、低比特数据通路 | 平台、频率、工作负载、精度、计时范围、综合或实测证据 |

数值结果按具体实验配置录入[结果表](data/measurements.csv)。不同论文的 images/s、tokens/s 与策略响应时延保留各自的工作负载和计时边界。

## 从这里开始

| 入口 | 用途 |
|---|---|
| [资料目录](data/papers.csv) | 可由 GitHub、Excel 或脚本读取的主目录：来源、发表场合、技术标签与阅读状态 |
| [会议与期刊入口](venues.md) | 固定关注范围、官方入口与搜索关键词 |
| [量化方法横向比较](comparisons/quantization.md) | 比较重构、缩放、旋转、时间步校准及部署条件 |
| [FPGA 架构横向比较](comparisons/architectures.md) | 比较硬件复用、流水线、存储；相关 HLS 工具作为实现参考 |
| [HG-PIPE 阅读示例](notes/hgpipe-2024.md) | 一篇论文如何拆成问题、证据、启发与待验证假设 |
| [2026 组会阅读安排](weekly/2026-09-14.md) | 新论文主读、针对性对照与组会问题 |
| [FPGA 架构汇报示例](talks/transformer-reuse-and-pipeline.md) | 一次架构方向的横向比较；量化方向也可独立组织汇报 |
| [维护方法](WORKFLOW.md) | 每周怎样增加资料、记录进度和更新比较 |
| [字段说明](data/README.md) | 论文级与实验配置级数据如何记录 |

## 两条主线与技术标签

| 主线 | 收录内容 |
|---|---|
| **量化算法** · `quantization` | 数值表示、权重重构、误差补偿、异常值、旋转、缩放、校准、混合精度与时间步相关量化 |
| **FPGA 架构** · `fpga-architecture` | 计算阵列、低比特数据通路、硬件复用、流水线、片上存储、外部存储与通信；相关 HLS、映射、调度和设计空间探索 |

目录中的 `tracks` 记录主线，交叉工作可以同时关联两条；`topics` 记录旋转、缓存、流水线、HLS 等具体技术。模型类别与 FPGA/GPU/ASIC 等平台属于应用或实现条件。评测方法服务于两条主线的证据比较。

## 第一轮要回答的问题

1. **量化算法**：重构、缩放和旋转分别改变了什么？多时间步统计如何影响校准？额外成本留在离线阶段还是推理阶段？
2. **FPGA 架构**：硬件复用与空间流水线各自节省什么、消耗什么？位宽、模型规模和访存条件如何改变选择？HLS 与映射方法怎样帮助实现这些选择？

## 新增资料的最小完成标准

- 在目录中留下稳定 ID、题名、正式来源、年份、技术标签和收录理由。
- 初筛后明确决定：待精读、仅作背景，或暂缓；已经读过的资料也可补登记。
- 精读时补一张[阅读卡片](templates/paper-note.md)，在相应比较表增加或修订一项判断。
- 数字结论必须能够追溯到原文表格或配置。不同硬件、精度、工作负载和计时范围的结果不直接排成速度榜。
- 保留开放问题：不确定的地方写出需要补查的证据或待做的实验。

## 组织方式参考

分类与论文索引参考 [Awesome Learning-based Odometry](https://github.com/KwanWaiPang/Awesome-Learning-based-VO-VIO)；横向比较字段参考 [Neural Network Accelerator Comparison](https://nicsefc.ee.tsinghua.edu.cn/projects/neural-network-accelerator.html)。完整元数据以[资料目录](data/papers.csv)为准，技术判断在对比表与阅读笔记中持续修订。
