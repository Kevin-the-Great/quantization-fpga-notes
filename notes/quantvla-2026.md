# QuantVLA

QuantVLA: Scale-Calibrated Post-Training Quantization for Vision-Language-Action Models

场合：**CVPR 2026**。状态：正式出版来源已核实。

核实的公开日期：**2026-02-23**（`arxiv_v1`）；查阅版本：arXiv v4 (2026-04-06); official CVPR proceedings metadata verified。来源核对：2026-09-14。

[论文入口](https://openaccess.thecvf.com/content/CVPR2026/html/Zhang_QuantVLA_Scale-Calibrated_Post-Training_Quantization_for_Vision-Language-Action_Models_CVPR_2026_paper.html) · [发表状态证据](https://openaccess.thecvf.com/content/CVPR2026/html/Zhang_QuantVLA_Scale-Calibrated_Post-Training_Quantization_for_Vision-Language-Action_Models_CVPR_2026_paper.html) · [日期证据](https://arxiv.org/abs/2602.20309) · [查阅的正文](https://arxiv.org/html/2602.20309v4) · [作者代码](https://github.com/AIoT-MLSys-Lab/QuantVLA)

本页是助理初筛与定向查阅记录；个人精读、复现与汇报均未登记。

## 机制与适用范围

用注意力温度与输出能量校准修正选择性 W4A8；扩散模块的注意力投影保留浮点。

Attention Temperature Matching 与 Output Head Balancing 分别用标量校准注意力分布和残差输出能量。

LLM 线性层与 DiT MLP 采用 W4A8；DiT Q/K/V/O 投影保留浮点。

选择性量化范围须与全权重量化分别比较；重参数化有前序方法来源。标量折叠方案不等于已经实现 FPGA 整数全图。

原文定位与证据范围：arXiv v4: §3.2–3.3、Fig. 2、§4；CVPR 正式论文页 39539–39549。

## 组会比较问题

注意力与残差接口的分布偏移是否比逐层 MSE 更能解释闭环误差？扩大量化覆盖后校准是否仍有效？

这是待验证的研究问题，不是原论文已经证明的迁移结论。

[返回量化比较](../comparisons/quantization.md) · [返回架构比较](../comparisons/architectures.md) · [English](../README.md) · [中文](../README.zh-CN.md)
