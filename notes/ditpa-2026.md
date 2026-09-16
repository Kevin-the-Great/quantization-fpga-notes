# DiTPA：动作规划中的冗余怎样影响加速架构

**下一次组会优先候选，待读。** 核对日期：2026-09-16。本次查阅为官方摘要、出版信息及作者代码；尚未取得全文，也未运行复现。

- 题名：DiTPA: A DiT-Based Action Planner Accelerator Exploiting Action-Denoising-Multimodality Redundancy for Embodied Artificial Intelligence
- 作者：Xin Zhao、Longke Yan、Jiancong Li、Yongkun Wu、Fengbin Tu。
- 出版：ISCA 2026，pp. 2709–2723，DOI [10.1109/ISCA66397.2026.00188](https://doi.org/10.1109/ISCA66397.2026.00188)。[官方议程](https://iscaconf.org/isca2026/program/)与 [HKUST 出版记录](https://researchportal.hkust.edu.hk/en/publications/ditpa-a-dit-based-action-planner-accelerator-exploiting-action-de/)均已核实。
- 日期：HKUST 记录 Published 2026-08-04；[Crossref](https://api.crossref.org/works/10.1109/ISCA66397.2026.00188) 的印刷出版日期为 2026-06。目录采用前者并注明来源，不声称它是最早公开日期。
- [作者仓库](https://github.com/fengbintu/ISCA2026-DiTPA)，查阅提交 `7be831e7aeb170c992a330ea995e8e0761966418`。
- 主线：FPGA 架构的可迁移方法参考；具体硬件平台待全文核实。量化与其冗余利用的相互影响是阅读问题。

## 为什么优先读

它把研究对象直接落在多模态 DiT 动作规划上。对我们最值得追的是：工作负载中哪些重复计算可以识别、怎样利用这些规律组织计算与缓存，以及闭环任务是否仍能成功。这个选题同时连接动作生成和未来加速器设计；模型名称不应替代机制分析。

## 已确认的机制

以下概括来自[作者机构收录的摘要](https://researchportal.hkust.edu.hk/en/publications/ditpa-a-dit-based-action-planner-accelerator-exploiting-action-de/)，公式、触发阈值、恢复路径及消融仍待全文。

| 冗余位置 | 作者提出的处理 | 精读时要画清楚 |
|---|---|---|
| 相邻动作 | 根据姿态变化预测、复用动作 | 判据使用什么信号，何时回到正常规划 |
| 去噪步骤 | 交替去噪与特征复用，用较低成本残差计算更新噪声 | 缓存什么、在哪一步刷新、怎样控制误差 |
| 多模态计算 | 根据模态有效周期与注意力稀疏性做经校准的近似计算 | 哪些张量可复用，哪些属于近似，生命周期有多长 |

摘要对应的硬件模块为动作预测器、可重配置 PE 阵列和多模态调度器；报告任务包含 LIBERO-Long。当前先整理机制，不摘录尚未核对配置的性能倍数。

## 公开代码能说明什么

下列事实均对应上面的固定提交；代码证据与论文完整实现范围分别记录。

| 已查证的事实 | 来源定位 | 对比较的影响 |
|---|---|---|
| 公开硬件评估读取软件结果并模拟硬件行为；quick evaluation 使用随附示例结果 | [README §2](https://github.com/fengbintu/ISCA2026-DiTPA/blob/7be831e7aeb170c992a330ea995e8e0761966418/README.md#L51-L53)、[§5](https://github.com/fengbintu/ISCA2026-DiTPA/blob/7be831e7aeb170c992a330ea995e8e0761966418/README.md#L357-L360) | 公开流程属于模拟评估，不能登记为 FPGA 上板复现；论文物理实现证据待全文确认 |
| 软件报告时延扣除了视觉/语言编码耗时及 other_latency | [编码计时](https://github.com/fengbintu/ISCA2026-DiTPA/blob/7be831e7aeb170c992a330ea995e8e0761966418/scripts/action_planner_modeling.py#L242-L263)、[扣除逻辑](https://github.com/fengbintu/ISCA2026-DiTPA/blob/7be831e7aeb170c992a330ea995e8e0761966418/scripts/ditpa_software_evaluation.py#L174-L186) | 该时延不能直接与完整视觉到动作的端到端时延对照 |
| 权重加载含整数值与尺度的反量化；代码另有有符号 8 bit 激活 QDQ，但有分支条件 | [权重处理](https://github.com/fengbintu/ISCA2026-DiTPA/blob/7be831e7aeb170c992a330ea995e8e0761966418/data_process/checkpoint_process.py#L98-L161)、[QDQ 与分支](https://github.com/fengbintu/ISCA2026-DiTPA/blob/7be831e7aeb170c992a330ea995e8e0761966418/scripts/modeling_llama.py#L201-L308) | 存在量化相关实现，不足以认定默认完整执行为 W4A8 或 W8A8；需追配置、checkpoint 和真实运算格式 |
| 性能及能耗由周期、频率、功率等模型参数计算，速度比含固定归一化系数 | [公式](https://github.com/fengbintu/ISCA2026-DiTPA/blob/7be831e7aeb170c992a330ea995e8e0761966418/scripts/ditpa_hardware_evaluation.py#L461-L500)、[参数](https://github.com/fengbintu/ISCA2026-DiTPA/blob/7be831e7aeb170c992a330ea995e8e0761966418/scripts/ditpa_hardware_evaluation.py#L569-L582) | 回查论文如何校准模型与归一化基线；保留模拟和测量的区别 |

当前作者仓库未找到 RTL、HLS、bitstream 或综合/布局布线报告；这只说明本次公开材料的范围，不能推出论文没有做相应验证。

## 与已有阅读怎样连接

| 对照 | 共同问题 | 这次要形成的判断 |
|---|---|---|
| [HG-PIPE](hgpipe-2024.md) | 计算依赖、缓存和执行组织 | 用它作架构背景，比较空间流水与跨动作/跨步骤复用分别需要什么状态；不直接比较吞吐数字 |
| [6Bit-Diffusion](6bit-diffusion-2026.md) | 跨去噪步骤的变化与特征缓存 | 若重点讲量化与复用，选它作定向对照；核对刷新、误差及省略计算，保留视频和机器人任务的差别 |
| [PTQ4DiT 的比较记录](../comparisons/quantization.md) | 时间步相关分布与校准 | 检查复用是否改变校准输入分布；量化与复用效果能否叠加需要实验 |

## 我们应追的三个问题

1. **量化会不会改变复用判定？** 保持同一动作与评测协议，比较浮点/量化 × 开启/关闭复用四种配置，观察判定分歧和闭环结果。这是拟议实验。
2. **少步采样后还剩多少可复用计算？** 逐项统计实际省略的调用、额外判定、刷新、缓存读写；不能把不同起点的加速倍数相乘。
3. **能否映射到计划中的 FPGA？** 从缓存容量、生命周期、读端口与不规则调度分析，再决定 PE 组织；当前尚无本项目实现证据。

[组会准备安排](../weekly/2026-09-16.md) · [汇报提纲](../talks/ditpa-2026.md) · [架构比较](../comparisons/architectures.md) · [English](../README.md) · [中文](../README.zh-CN.md)
