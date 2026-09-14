# 量化方法：问题、机制与部署前提

更新：2026-09-14。本文由助理根据一手论文和作者代码仓库整理，属于**初筛与定向查阅笔记**；不表示用户已精读、复现或完成组会汇报。这 7 篇用于建立比较方法，不构成最新工作全集或 SOTA 排名。

收录依据是方法解决的技术问题及其迁移价值，当前可结合 VLM、迭代生成和 VLA 验证。模型名称是适用范围信息，不作为收录边界。表中的“待验证问题”是我们的研究问题，不是原论文结论。

W/A/KV 分别表示权重、激活和 KV 表示位宽。位宽标签仅描述论文所比较的配置；不自动覆盖嵌入、归一化、Softmax、重缩放或每一次矩阵乘法的实际计算精度。不同配置应分别登记。

## 第一组：重构与通道缩放

| 稳定 ID / 论文 | 解决什么问题 | 方法机制 | 代表精度与范围 | 校准与运行时前提 | 待验证的迁移问题 | 一手来源定位 |
| --- | --- | --- | --- | --- | --- | --- |
| `gptq-2023` / GPTQ，ICLR 2023 | 大模型低比特权重量化的逐层误差，以及二阶方法的处理成本 | 以层输出重构为目标，利用校准输入的二阶信息补偿量化误差；批量更新与 Cholesky 重写提高可扩展性 | **Weight-only**，代表配置 W4A16、W3A16；本身不提供 A8 校准方法 | 需要代表性层输入来估计二阶信息；原始权重压缩及对应内核不能直接证明整数 W4×A8 加速 | 更换输入分布、旋转或缩放后，二阶统计是否仍匹配？局部重构改善能否保留到整个迭代模型的任务结果？ | [正文](https://arxiv.org/html/2210.17323v2)：§3–4、Algorithm 1；[作者仓库](https://github.com/IST-DASLab/gptq) |
| `smoothquant-2023` / SmoothQuant，ICML 2023 | 激活异常值使低比特激活难以量化 | 通过等价的通道缩放，将部分激活量化难度转移给权重；可将缩放折叠到相邻算子参数 | 代表配置 **W8A8**；不是原论文已经解决 W4A8 的证据 | 需要离线激活统计；O1 为逐 token 动态、O2 为逐 tensor 动态、O3 为逐 tensor 静态；轻量算子仍保留浮点 | 权重降至 W4 后，迁移强度如何取舍？静态尺度在视觉、语言及不同时间步上是否稳定？哪些缩放能在目标图中折叠？ | [正式论文](https://proceedings.mlr.press/v202/xiao23c/xiao23c.pdf)：§4、Table 2 |
| `awq-2024` / AWQ，MLSys 2024 | 少量重要权重对低比特误差敏感；仅看权重幅值不能充分判断重要性 | 由输入激活判断重要通道，搜索通道缩放以保护对应权重的量化精度 | **Weight-only**，代表配置 W4A16、W3A16；不能记为 W4A8；包含 LLM 与多模态语言模型验证 | 使用小校准集搜索缩放，不依赖反向传播或逐权重重构；TinyChat 内核将低比特权重在线反量化后计算 | 激活感知的重要性在不同模态中是否稳定？增加 A8 后，原来的缩放目标是否仍合适？存储节约如何转化为目标 FPGA 的计算收益？ | [正式论文页](https://proceedings.mlsys.org/paper_files/paper/2024/hash/42a452cbafa9dd64e9ba4aa95cc1ef21-Abstract-Conference.html)；[作者正文版本](https://arxiv.org/html/2306.00978v5)：§3.2、§4.2、§5.1 |

## 第二组：旋转与低比特表示

| 稳定 ID / 论文 | 解决什么问题 | 方法机制 | 代表精度与范围 | 校准与运行时前提 | 待验证的迁移问题 | 一手来源定位 |
| --- | --- | --- | --- | --- | --- | --- |
| `quarot-2024` / QuaRot，NeurIPS 2024 | 隐藏状态、层内激活和 KV 的异常值限制低比特表示 | 利用计算等价性引入旋转；部分旋转吸收到权重，部分通过在线 Hadamard 变换实现；随后量化 | 代表配置 **W4A4KV4**；实现中的 attention 保留 FP16 query，KV 加载后反量化并进行 FP16 点积，因此不能把标签解释为全图 INT4 计算 | 常用 W4 配置结合 GPTQ；线性层输入使用动态逐 token 量化；在线旋转、归一化和反量化仍需要实现与计时 | 目标结构是否满足旋转等价条件？保留的在线变换需要多少带宽和计算？VLA 中 K/V 的生命周期是否支持同样的缓存收益？ | [正式论文](https://proceedings.neurips.cc/paper_files/paper/2024/file/b5b939436789f76f08b9d0da5e81af7c-Paper-Conference.pdf)：§4，Stage 2a–2c；§5 |
| `spinquant-2025` / SpinQuant，ICLR 2025 | 随机旋转的量化效果可能随旋转选择显著变化 | 冻结预训练权重，以校准损失学习正交旋转，采用 Cayley 优化；再执行量化 | 包含 W4A8 与 **W4A4KV4** 等配置；必须同时记录 `no_had` / `had` 变体 | 需要带梯度的离线旋转优化；`no_had` 使用可合并旋转，`had` 额外保留在线 Hadamard；学习旋转与后续 GPTQ 是不同阶段 | 当前任务应选择语言损失、层输出损失还是动作相关损失？收益是否值得额外校准成本？目标架构能否满足折叠条件？ | [ICLR 正式论文](https://openreview.net/pdf/df22ed66a962124d235f9d8c773a13f8f43e109a.pdf)；[作者正文版本](https://arxiv.org/html/2405.16406v3)：§3–4；[作者仓库](https://github.com/facebookresearch/SpinQuant) |

## 第三组：迭代生成中的校准

| 稳定 ID / 论文 | 解决什么问题 | 方法机制 | 代表精度与范围 | 校准与运行时前提 | 待验证的迁移问题 | 一手来源定位 |
| --- | --- | --- | --- | --- | --- | --- |
| `qdiffusion-2023` / Q-Diffusion，ICCV 2023 | 迭代去噪误差累积、跨时间步分布变化，以及 U-Net 拼接分支的分布差异 | 跨时间步抽样构建校准集，做块级重构；对 shortcut 拼接的不同分支分别量化 | 包含 **W4A8、W8A8** 及仅权重量化配置；对象主要是图像扩散模型的去噪网络 | 校准样本来自浮点生成轨迹；块重构和激活步长校准均需离线处理；时间步感知的采样不等于每步独立量化尺度 | 迭代动作生成应怎样覆盖状态与时间步？减少采样步数后是否重校准？没有 U-Net 拼接结构时，分支拆分机制是否仍有对应对象？ | [作者正文](https://arxiv.org/html/2302.04304v3)：§3.1–3.3、Algorithm 1、§4；[ICCV 论文页](https://openaccess.thecvf.com/content/ICCV2023/html/Li_Q-Diffusion_Quantizing_Diffusion_Models_ICCV_2023_paper.html) |
| `ptq4dit-2024` / PTQ4DiT，NeurIPS 2024 | DiT 显著通道的极值，以及这些通道随时间步变化的分布 | 通道显著性平衡（CSB）；利用 Spearman 相关性加权聚合跨步显著性（SSC）；离线重参数化 | **W8A8、W4A8**；在图像生成 DiT 上验证，不能直接等同于动作生成 DiT | 从多个时间步采集校准输入，离线聚合统计；通过 adaLN 等位置折叠缩放；SSC 不是推理时在线计算 Spearman 相关性 | 动作条件和少步生成下，跨步显著性规律是否一致？统一聚合与分步尺度如何比较？目标 adaLN/注意力结构是否允许同样的重参数化？ | [正式论文页](https://proceedings.neurips.cc/paper_files/paper/2024/hash/72d32f4fe0b7af03732bd227bf1c4a5f-Abstract-Conference.html)；[作者正文](https://arxiv.org/html/2405.16005v3)：§4.1–4.3、§5.1、Appendix B |

## 怎样把比较转化为自己的阅读问题

这三组方法可以作用于不同阶段，不能只按“谁的位宽更低”排成单一榜单。下一轮精读优先检查下面三个问题；这里提出的是比较任务，尚未给出实验证据。

1. **重构目标与实际使用是否一致？** 对照 GPTQ、AWQ：各自使用什么输入统计，优化什么对象，哪些校准选择需要在新任务中重新验证？
2. **改善分布需要哪些在线代价？** 对照 SmoothQuant、QuaRot、SpinQuant：逐项画出可折叠变换、在线变换、动态尺度与浮点算子，作为后续硬件映射的输入。
3. **时间步信息应该进入哪里？** 对照 Q-Diffusion、PTQ4DiT：区分校准样本覆盖、跨步统计聚合、分步尺度表和在线动态量化；它们是不同设计选择。

补充定量结果时，每一行对应一个明确实验配置，并至少关联模型版本、数据集、量化范围及例外、尺度粒度、校准协议、评测协议和原文表号。语言困惑度、图像 FID 与闭环机器人成功率应分别比较；软件量化结果也应与真实整数内核、综合结果及上板性能分开记录。

## 完整题名与出版年份

此处年份为正式发表年份，与预印本首次上传年份可能不同。

| ID | 完整题名 | 正式发表 |
| --- | --- | --- |
| `gptq-2023` | GPTQ: Accurate Post-Training Quantization for Generative Pre-trained Transformers | ICLR 2023 |
| `smoothquant-2023` | SmoothQuant: Accurate and Efficient Post-Training Quantization for Large Language Models | ICML 2023 |
| `awq-2024` | AWQ: Activation-aware Weight Quantization for On-Device LLM Compression and Acceleration | MLSys 2024 |
| `quarot-2024` | QuaRot: Outlier-Free 4-Bit Inference in Rotated LLMs | NeurIPS 2024 |
| `spinquant-2025` | SpinQuant: LLM Quantization with Learned Rotations | ICLR 2025 |
| `qdiffusion-2023` | Q-Diffusion: Quantizing Diffusion Models | ICCV 2023 |
| `ptq4dit-2024` | PTQ4DiT: Post-training Quantization for Diffusion Transformers | NeurIPS 2024 |
