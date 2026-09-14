# OrbitQuant

OrbitQuant: Data-Agnostic Quantization for Image and Video Diffusion Transformers

场合：**arXiv**。状态：预印本；本次未核实正式录用信息。

核实的公开日期：**2026-07-02**（`arxiv_v1`）；查阅版本：v1 (2026-07-02)。来源核对：2026-09-14。

[论文入口](https://arxiv.org/abs/2607.02461) · [发表状态证据](https://arxiv.org/abs/2607.02461) · [日期证据](https://arxiv.org/abs/2607.02461) · [查阅的正文](https://arxiv.org/html/2607.02461v1)

本页是助理初筛与定向查阅记录；个人精读、复现与汇报均未登记。

## 机制与适用范围

在归一化后的随机置换分块 Hadamard 基底中量化权重和激活，同维度共享 Lloyd–Max 码本，减少对任务数据范围校准的依赖。

W4A4 / W2A4 and lower-bit experiments; these are nonuniform codebook indices, not an INT4 GEMM specification.

无需校准不等于没有在线代价：激活范数、前向旋转和码本处理仍需实现；权重行范数为 BF16。需查 Appendix B.2 的跳过层及 AdaLN 配置，不能称全图统一低比特整数。

原文定位与证据范围：Sections 4.1–4.5; Table 1; Section 5.5; Section 6.2; Appendix B.2 and D.

## 组会比较问题

非均匀码本与在线归一化在 FPGA 上的代价，是否值得替换现有均匀静态 W4A8 表示？

这是待验证的研究问题，不是原论文已经证明的迁移结论。

[返回量化比较](../comparisons/quantization.md) · [返回架构比较](../comparisons/architectures.md) · [English](../README.md) · [中文](../README.zh-CN.md)
