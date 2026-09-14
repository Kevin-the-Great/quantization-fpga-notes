# HGQ-LUT

HGQ-LUT: Fast LUT-Aware Training and Efficient Architectures for DNN Inference

场合：**FCCM 2026**。状态：已由官方议程确认录用；本次未进一步核实正式论文集出版状态。

核实的公开日期：**2026-04-24**（`arxiv_v1`）；查阅版本：arXiv v1。来源核对：2026-09-14。

[论文入口](https://arxiv.org/abs/2604.22293) · [发表状态证据](https://www.fccm.org/fccm-26-program/) · [日期证据](https://arxiv.org/abs/2604.22293) · [查阅的正文](https://arxiv.org/html/2604.22293v1) · [作者代码](https://github.com/calad0i/HGQ-LUT-AE)

本页是助理初筛与定向查阅记录；个人精读、复现与汇报均未登记。

## 机制与适用范围

用张量算子训练 LUT-Dense/LUT-Conv，以细粒度位宽、剪枝及 LUT 代价近似共同优化，可生成查表与常规算术混合架构。硬件指标来自 OOC 布局布线报告。

原文定位与证据范围：全文 https://arxiv.org/html/2604.22293v1：§III 量化/架构；§IV 生成与位精确验证；§V-A 明确 xcvu13p、Vivado2025.1、out-of-context post-routing reports；Table II–III 的纳秒值与Fmax不应写成整机上板端到端实测。作者 artifact https://github.com/calad0i/HGQ-LUT-AE。

## 组会比较问题

资源目标是否能迁移到我们需要复用的较大矩阵核？在小算子上采用逻辑查表是否有收益？训练成本、表规模和大模型精度应分别验证。

这是待验证的研究问题，不是原论文已经证明的迁移结论。

[返回量化比较](../comparisons/quantization.md) · [返回架构比较](../comparisons/architectures.md) · [English](../README.md) · [中文](../README.zh-CN.md)
