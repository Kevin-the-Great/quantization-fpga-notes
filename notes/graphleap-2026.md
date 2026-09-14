# GraphLeap

GraphLeap: Decoupling Graph Construction and Convolution for Vision GNN Acceleration on FPGA

场合：**FCCM 2026**。状态：已由官方议程确认录用；本次未进一步核实正式论文集出版状态。

核实的公开日期：**2026-04-23**（`arxiv_v1`）；查阅版本：arXiv v1。来源核对：2026-09-14。

[论文入口](https://arxiv.org/abs/2604.21290) · [发表状态证据](https://www.fccm.org/fccm-26-program/) · [日期证据](https://arxiv.org/abs/2604.21290) · [查阅的正文](https://arxiv.org/html/2604.21290v1) · [作者代码](https://github.com/anvitha305/GraphLeap)

本页是助理初筛与定向查阅记录；个人精读、复现与汇报均未登记。

## 机制与适用范围

把建图所用特征提前一层，使建图引擎与特征更新引擎并行，通过流式连接减少边特征物化。该改写改变模型，需要微调和精度核验。

原文定位与证据范围：全文 https://arxiv.org/html/2604.21290v1：§III–IV 是算法/架构；§IV-H 与 Table II 报 U280 post-route 300 MHz；§V-A4 报硬件计数器采集 batch1 延迟并含 H2D/D2H。Table V 微调后数值仍低于原模型，不能称完全恢复。具体运算精度需继续核实；本次未独立复现。

## 组会比较问题

哪些依赖可等价重排，哪些需要使用旧特征而改变算法？若用于迭代推理，省下的等待能否超过额外缓冲，且闭环精度是否可接受？

这是待验证的研究问题，不是原论文已经证明的迁移结论。

[返回量化比较](../comparisons/quantization.md) · [返回架构比较](../comparisons/architectures.md) · [English](../README.md) · [中文](../README.zh-CN.md)
