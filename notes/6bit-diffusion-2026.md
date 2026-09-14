# 6Bit-Diffusion

6Bit-Diffusion: Inference-Time Mixed-Precision Quantization for Video Diffusion Models

场合：**arXiv**。状态：预印本；本次未核实正式录用信息。

核实的公开日期：**2026-03-19**（`arxiv_v1`）；查阅版本：v1 (2026-03-19)。来源核对：2026-09-14。

[论文入口](https://arxiv.org/abs/2603.18742) · [发表状态证据](https://arxiv.org/abs/2603.18742) · [日期证据](https://arxiv.org/abs/2603.18742) · [查阅的正文](https://arxiv.org/html/2603.18742v1)

本页是助理初筛与定向查阅记录；个人精读、复现与汇报均未登记。

## 机制与适用范围

根据前一步的块输入输出差异预测敏感性，在 NVFP4 与 INT8 激活间切换，并结合残差缓存与刷新控制。

NVFP4 weights; activations dynamically routed to NVFP4 or INT8. The INT8 route casts stored weights to INT8 for GEMM. The title does not mean a native uniform six-bit datapath.

需要校准拟合、在线统计与 Hadamard，以及对应混合格式内核；实验使用 RTX 5090。总加速包含缓存跳算，应与纯量化分开记录，不能平移为 FPGA W4A8 加速。

原文定位与证据范围：Sections 4.1–4.3; Section 5.1; Section 5.3; Table 3 ablations.

## 组会比较问题

少步动作生成中是否仍存在足够稳定的跨步关系，且动态切换收益能覆盖控制与转换成本？

这是待验证的研究问题，不是原论文已经证明的迁移结论。

[返回量化比较](../comparisons/quantization.md) · [返回架构比较](../comparisons/architectures.md) · [English](../README.md) · [中文](../README.zh-CN.md)
