# ReSpinQuant

ReSpinQuant: Efficient Layer-Wise LLM Quantization via Subspace Residual Rotation Approximation

场合：**ICML 2026**。状态：已由官方议程确认录用；本次未进一步核实正式论文集出版状态。

核实的公开日期：**2026-04-13**（`arxiv_v1`）；查阅版本：v2 (2026-05-28)。来源核对：2026-09-14。

[论文入口](https://arxiv.org/abs/2604.11080) · [发表状态证据](https://icml.cc/virtual/2026/poster/61314) · [日期证据](https://arxiv.org/abs/2604.11080) · [查阅的正文](https://arxiv.org/html/2604.11080v2)

本页是助理初筛与定向查阅记录；个人精读、复现与汇报均未登记。

## 机制与适用范围

学习逐层正交旋转并折叠进权重，用低秩子空间修正处理残差路径的基底不一致，验证 W4A4KV4 和 W3A3KV3 等配置。

Main configurations W4A4KV4 / W3A3KV3; Appendix B.2 adds W4A8KV8 and W4A8KV16.

仍需离线梯度优化与在线残差修正；低秩近似并非严格无损折叠。作者比较对若干基线统一修改了 clipping/GPTQ 配置，不能当作这些方法各自最佳配方的排名。GPU 结果不能直接当作 FPGA 证据。

原文定位与证据范围：arXiv v2: Section 3; Section 4.1; Table 1; Section 4.4/Table 5; Appendix A and B.2.

## 组会比较问题

对目标残差结构，低秩基底修正的精度收益能否覆盖新增计算、缓冲与校准成本？

这是待验证的研究问题，不是原论文已经证明的迁移结论。

[返回量化比较](../comparisons/quantization.md) · [返回架构比较](../comparisons/architectures.md) · [English](../README.md) · [中文](../README.zh-CN.md)
