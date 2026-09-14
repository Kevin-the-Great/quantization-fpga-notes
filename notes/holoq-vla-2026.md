# HoloQ-VLA

HoloQ-VLA: Uniform W4A4 Quantization of Vision-Language-Action Models

场合：**arXiv**。状态：预印本；本次未核实正式录用信息。

核实的公开日期：**2026-05-27**（`arxiv_v1`）；查阅版本：v3 (2026-08-11)。来源核对：2026-09-14。

[论文入口](https://arxiv.org/abs/2605.28803) · [发表状态证据](https://arxiv.org/abs/2605.28803) · [日期证据](https://arxiv.org/abs/2605.28803) · [查阅的正文](https://arxiv.org/html/2605.28803v3)

本页是助理初筛与定向查阅记录；个人精读、复现与汇报均未登记。

## 机制与适用范围

复合旋转与逐时间步尺度，用于语言与扩散动作模块的 W4A4；8 月 11 日更新至 v3。

权重相关的分块 SVD 旋转结合 Hadamard 与通道重排；激活使用静态逐时间步、逐通道尺度。

报告语言模块和扩散动作头的 W4A4，包括注意力；该标签不证明全部辅助算子或真实设备全图采用整数计算。

改变采样步数后需要重新检查尺度表适用性；在线旋转、尺度处理与真实低比特内核仍需分别核验。

原文定位与证据范围：arXiv v3: Methodology；Appendix 的 Key Hyperparameters / Table 5。

## 组会比较问题

逐步静态尺度相对跨步聚合能带来多少收益？改变动作条件和采样步数后是否仍稳定？

这是待验证的研究问题，不是原论文已经证明的迁移结论。

## 版本说明

v1 于 2026-05-27 公开，原题为 Ω-QVLA: Robust Quantization for Vision-Language-Action Models via Composite Rotation and Per-step Scaling；v3 于 2026-08-11 更新为当前题名，作为同一项工作去重。

[返回量化比较](../comparisons/quantization.md) · [返回架构比较](../comparisons/architectures.md) · [English](../README.md) · [中文](../README.zh-CN.md)
