# LUT-LLM

LUT-LLM: Efficient Language Model Inference with Memory-based Computations on FPGAs

场合：**FCCM 2026**。状态：已由官方议程确认录用；本次未进一步核实正式论文集出版状态。

核实的公开日期：**2025-11-09**（`arxiv_v1`）；查阅版本：arXiv v2 (2026-03-22)。来源核对：2026-09-14。

[论文入口](https://arxiv.org/html/2511.06174v2) · [发表状态证据](https://www.fccm.org/fccm-26-program/) · [日期证据](https://arxiv.org/abs/2511.06174)

本页是助理初筛与定向查阅记录；个人精读、复现与汇报均未登记。

## 机制与适用范围

激活与权重向量联合量化，结合带宽感知质心搜索、二维查表和混合执行；需要模型转换与训练。

原文定位与证据范围：arXiv v2 §§III, IV-B–IV-E; §V-A, Fig. 10 and §V-C. Qwen3-1.7B prototype on AMD V80 at 250 MHz, using Vitis HLS/TAPA/RapidStream. Allo/InTAR/FlightLLM comparison latencies on the target model are simulator-derived (§V-A), not direct reruns of those accelerators. Official artifact provides bitstream, timing report and ILA-cycle-based latency instructions: https://github.com/LUT-FPGA/LUT-LLM . No local reproduction.

## 组会比较问题

码本训练与在线质心搜索的代价是否值得？目标激活分布和矩阵形状能否支持该查表容量/带宽平衡？INT8 查表项不能等同于标量 W8A8 或全图整数计算。

这是待验证的研究问题，不是原论文已经证明的迁移结论。

## 版本说明

FCCM 2026 议程题名省略了 “Large”，arXiv 摘要元数据仍保留该词。arXiv v1 为 2025-11-09，v2 为 2026-03-22；本库按 2026 年录用工作归组，同时保留其 2025 年下半年预印本日期。

[返回量化比较](../comparisons/quantization.md) · [返回架构比较](../comparisons/architectures.md) · [English](../README.md) · [中文](../README.zh-CN.md)
