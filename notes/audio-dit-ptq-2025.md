# Audio DiT PTQ

Post-Training Quantization for Audio Diffusion Transformers

场合：**WASPAA 2025**。状态：已由官方议程确认录用；本次未进一步核实正式论文集出版状态。

核实的公开日期：**2025-09-30**（`arxiv_v1`）；查阅版本：v1 (2025-09-30)。来源核对：2026-09-14。

[论文入口](https://arxiv.org/abs/2510.00313) · [发表状态证据](https://waspaa.com/author-index/) · [日期证据](https://arxiv.org/abs/2510.00313) · [查阅的正文](https://arxiv.org/html/2510.00313v1)

本页是助理初筛与定向查阅记录；个人精读、复现与汇报均未登记。

## 机制与适用范围

在音频 DiT 上比较静态与时间步感知缩放，并评估 FP16 低秩分支补偿量化误差的效果。

Abstract/body describe W8A8 and W4A8, but arXiv v1 Table 1 labels the low-bit rows W4A4. Keep this inconsistency explicit and do not extract low-bit quantitative results until resolved.

FP16 补偿分支保留高精度计算；作者报告动态路径更慢。正文与 Table 1 的 A8/A4 标注矛盾未解决，因此仅收定性机制，不进入数值榜单。

原文定位与证据范围：Section 2, Eqs. 6–9; Section 4/Table 1; Section 5.

## 组会比较问题

分步分通道尺度带来的精度改善能否通过离线表与硬件调度保留，并避免动态处理开销？

这是待验证的研究问题，不是原论文已经证明的迁移结论。

[返回量化比较](../comparisons/quantization.md) · [返回架构比较](../comparisons/architectures.md) · [English](../README.md) · [中文](../README.zh-CN.md)
