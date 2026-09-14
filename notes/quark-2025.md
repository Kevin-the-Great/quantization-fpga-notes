# QUARK

QUARK: Quantization-Enabled Circuit Sharing for Transformer Acceleration by Exploiting Common Patterns in Nonlinear Operations

场合：**ICCAD 2025**。状态：正式出版来源已核实。

核实的公开日期：**2025-11-10**（`arxiv_v1`）；查阅版本：arXiv v1。来源核对：2026-09-14。

[论文入口](https://arxiv.org/html/2511.06767v1) · [发表状态证据](https://doi.org/10.1109/ICCAD66269.2025.11240736) · [日期证据](https://arxiv.org/abs/2511.06767)

本页是助理初筛与定向查阅记录；个人精读、复现与汇报均未登记。

## 机制与适用范围

通过量化与近似使 Softmax、GELU、LayerNorm 共享子算子；比较整数近似、分组尺度对齐和共享电路对流水并发的影响。

原文定位与证据范围：arXiv v1 §III, §IV-A (Fig. 5), §IV-B (Fig. 6), §V-A and §V-C (Tables III–IV). ZCU102 at 300 MHz; hardware resources explicitly derive from place-and-route. Reported latency should retain author-reported status; the methods text inspected does not independently establish a board-timed end-to-end measurement. Accuracy tables cover W8A8/W6A6/W4A4. No local reproduction.

## 组会比较问题

Softmax、GELU、LayerNorm 的共享逻辑是否适合目标算子集合？时分复用与现有模块并发是否冲突？重排、分组尺度和整数近似能否保持迭代动作生成精度？

这是待验证的研究问题，不是原论文已经证明的迁移结论。

## 版本说明

Exact first public full-manuscript date is unconfirmed. arXiv v1 is 2025-11-10; ICCAD's official program lists the presentation on 2025-10-29. Do not substitute that presentation date for a full-paper release date.

[返回量化比较](../comparisons/quantization.md) · [返回架构比较](../comparisons/architectures.md) · [English](../README.md) · [中文](../README.zh-CN.md)
