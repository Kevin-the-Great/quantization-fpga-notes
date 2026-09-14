# MXFP PTQ Benchmark

Benchmarking Post-Training Quantization of Large Language Models under Microscaling Floating Point Formats

场合：**ACL 2026**。状态：正式出版来源已核实。

核实的公开日期：**2026-01-14**（`arxiv_v1`）；查阅版本：arXiv v1 (2026-01-14); official ACL metadata verified。来源核对：2026-09-14。

[论文入口](https://aclanthology.org/2026.acl-long.1854/) · [发表状态证据](https://aclanthology.org/2026.acl-long.1854/) · [日期证据](https://arxiv.org/abs/2601.09555) · [查阅的正文](https://arxiv.org/html/2601.09555v1)

本页是助理初筛与定向查阅记录；个人精读、复现与汇报均未登记。

## 机制与适用范围

比较多种 PTQ 方法在 MXFP 下的行为，分析共享尺度的量化误差及预缩放策略，并拆分多模态模型中的模块敏感性。

MXFP W8A8 / W4A8 / W4A4 and supplementary configurations; format must accompany bit-width.

MXFP4 与 INT4、NVFP4 不是同一表示；论文限于所测 7B/8B 级模型及 MXFP，不能把模块敏感性结论推广为所有 VLA 的规律。预缩放策略有前序来源，不应宣称全部为本文首创。

原文定位与证据范围：arXiv Sections 3.2–3.5; Tables 3–6; Limitations; ACL Anthology bibliographic fields.

## 组会比较问题

在同一模型与任务上，算法、量化粒度和共享尺度格式各自造成多大影响？

这是待验证的研究问题，不是原论文已经证明的迁移结论。

[返回量化比较](../comparisons/quantization.md) · [返回架构比较](../comparisons/architectures.md) · [English](../README.md) · [中文](../README.zh-CN.md)
