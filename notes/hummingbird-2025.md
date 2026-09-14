# Hummingbird

Hummingbird: A Smaller and Faster Large Language Model Accelerator on Embedded FPGA

场合：**ICCAD 2025**。状态：正式出版来源已核实。

核实的公开日期：**2025-07-04**（`arxiv_v1`）；查阅版本：arXiv v2。来源核对：2026-09-14。

[论文入口](https://arxiv.org/html/2507.03308v2) · [发表状态证据](https://ieeexplore.ieee.org/document/11241002/) · [日期证据](https://arxiv.org/abs/2507.03308)

本页是助理初筛与定向查阅记录；个人精读、复现与汇报均未登记。

## 机制与适用范围

面向嵌入式 FPGA 的 DSP 高效 DOT/AXPY GEMV、DDR 列对齐访问与 GQA 缓冲；区分权重压缩格式和实际计算精度。

原文定位与证据范围：arXiv v2 §§III, IV-A–IV-E; Table I; Fig. 7, 9, 11; §V-A and Tables IV–V. KV260/ZCU104/U250 deployment and decoding results are reported; resource and power figures come from Vivado reports, so power is not established as board-meter measurement. No local reproduction.

## 组会比较问题

在目标 GEMM 形状和访存系统下，哪些 DSP 与数据复用机制仍适用？W4 权重和 KV8 不代表 W4A8 计算：正文 VPU 使用 INT24、SPU 使用 FP16；DDR 优化也不能直接套到 HBM。

这是待验证的研究问题，不是原论文已经证明的迁移结论。

## 版本说明

first_public_date uses the verified arXiv v1 manuscript date; IEEE Xplore added the ICCAD paper on 2025-11-20. This is not the date of its earlier DATE predecessor.

[返回量化比较](../comparisons/quantization.md) · [返回架构比较](../comparisons/architectures.md) · [English](../README.md) · [中文](../README.zh-CN.md)
