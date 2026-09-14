# 文献来源与会议期刊关注表

入口与研究范围核验日期：**2026-09-14**。这是持续阅读的来源表；年度程序页、论文集、征稿范围需要逐届更新。表中的年份表示已核验的入口实例，不表示只读该年论文，也不表示某届未来日程或录用结果。

本库始终围绕**量化算法**和**FPGA 架构**两条主线组织资料。前者关注量化误差、校准与数据表示，后者关注计算单元、数据流、存储和资源组织；HLS 与相关工具作为 FPGA 架构的实现支撑。VLA、LLM、ViT、扩散模型等作为应用标签，资料来源保持开放：只要能回答这两条主线中的具体问题，就可以纳入。会议和期刊帮助发现论文，问题分类把不同来源的工作放到一起比较。

## 1. 从问题找到来源

| 当前要理解的问题 | 优先入口 | 阅读时提取的内容 |
|---|---|---|
| 量化算法：低比特为何损失精度，怎样校准、重构或变换分布 | ICLR、ICML、NeurIPS；CVPR、ICCV、ECCV 中相关论文 | 误差目标、权重与激活精度、异常值处理、校准数据、静态/动态尺度、额外在线算子 |
| 两条主线的联系：量化格式怎样适配 FPGA 数据通路 | MLSys；FPGA、FCCM、FPL；DAC、ICCAD、DATE、ASP-DAC | 数据格式、解包与缩放、算子融合、批量与形状、内存流量、计时边界 |
| FPGA 架构：计算阵列、流水线和存储怎样协同 | FPGA、FCCM、FPL；EDA 会议和硬件期刊 | 数据复用、计算单元复用、片上缓冲、外存带宽、低比特打包、资源与频率 |
| FPGA 架构的实现：怎样利用 HLS 和工具落实设计 | FPGA、FCCM、FPL；DAC、ICCAD、DATE、ASP-DAC、TCAD；相关 PLDI 论文 | 调度、资源绑定、存储划分、数据流构造、DSL/编译器、设计空间探索 |
| 两条主线中遇到的具体瓶颈需要借鉴其他架构或软硬件接口 | 按需补充 MICRO、HPCA、ISCA、ASPLOS | 与量化或 FPGA 设计直接相关的内存、执行、调度和接口机制，以及迁移条件 |

例如，SmoothQuant 的通道缩放来自 LLM 量化，HG-PIPE 的流水线来自 ViT 加速；两者能否用于另一个模型，要根据结构与代价判断。筛选时无需要求论文标题出现 VLA。

## 2. 量化算法主线的主要入口

| 来源 | 本库重点关注 | 官方论文集 / 程序入口 |
|---|---|---|
| **ICLR** | PTQ/QAT、误差重构、旋转与重参数化、校准、低比特学习 | [ICLR 论文集](https://proceedings.iclr.cc/)；旧届论文从相应年度会议页或 OpenReview 的会议分组查找 |
| **ICML** | 通用量化方法、优化目标、误差分析、量化的精度与计算代价 | [PMLR 论文集总目录](https://proceedings.mlr.press/)，选择明确标注 ICML 的年度卷；PMLR 也收录其他会议与 workshop |
| **NeurIPS** | 低比特表示、旋转、重构、混合精度、扩散模型量化 | [NeurIPS 论文集](https://proceedings.neurips.cc/)，区分主会与其他 track |
| **MLSys** | 量化算法与部署格式、低比特内核的联系；量化带来的内存、带宽与运行时变化 | [MLSys 论文集](https://proceedings.mlsys.org/) |
| **CVPR** | ViT/VLM/DiT 的量化，视觉模块和非线性算子的量化处理 | [CVF 开放论文库](https://openaccess.thecvf.com/)，选择 CVPR 对应年份 |
| **ICCV** | 视觉网络量化、扩散模型时间步校准、激活分布与任务影响 | [CVF 开放论文库](https://openaccess.thecvf.com/)，选择 ICCV 对应年份 |
| **ECCV** | ViT 与扩散模型量化、非均匀表示、量化误差与任务影响 | [ECVA 论文目录](https://www.ecva.net/papers.php) |

这组入口有明确的已发表实例：[GPTQ（ICLR 2023，作者机构记录）](https://www.research-collection.ethz.ch/items/00736213-37b2-4e99-b015-141349b71413)、[SmoothQuant（ICML 2023）](https://proceedings.mlr.press/v202/xiao23c.html)、[AWQ（MLSys 2024）](https://proceedings.mlsys.org/paper_files/paper/2024/hash/42a452cbafa9dd64e9ba4aa95cc1ef21-Abstract-Conference.html)、[QuaRot（NeurIPS 2024）](https://proceedings.neurips.cc/paper_files/paper/2024/hash/b5b939436789f76f08b9d0da5e81af7c-Abstract-Conference.html)、[SpinQuant（ICLR 2025）](https://proceedings.iclr.cc/paper_files/paper/2025/hash/e5b1c0d4866f72393c522c8a00eed4eb-Abstract-Conference.html)。它们分别提供权重重构、通道缩放、激活感知权重量化和旋转等思路，原始对象均不是 VLA。

视觉与扩散模型也跨越这些来源：[Q-Diffusion（ICCV 2023）](https://openaccess.thecvf.com/content/ICCV2023/html/Li_Q-Diffusion_Quantizing_Diffusion_Models_ICCV_2023_paper.html)、[PTQ4DiT（NeurIPS 2024）](https://proceedings.neurips.cc/paper_files/paper/2024/hash/72d32f4fe0b7af03732bd227bf1c4a5f-Abstract-Conference.html)、[Q-DiT（CVPR 2025）](https://openaccess.thecvf.com/content/CVPR2025/papers/Chen_Q-DiT_Accurate_Post-Training_Quantization_for_Diffusion_Transformers_CVPR_2025_paper.pdf)、[AdaLog（ECCV 2024）](https://eccv.ecva.net/virtual/2024/poster/813)。因此不能把“DiT 量化”或“旋转量化”固定绑定到单一会议。

## 3. FPGA 架构主线的主要入口

下列会议也覆盖设计自动化和系统工具；本库从中选择服务于 FPGA 架构设计与实现的工作。

| 来源 | 本库重点关注 | 官方程序 / 论文入口 | 范围与使用说明 |
|---|---|---|---|
| **FPGA / ISFPGA** | FPGA 计算架构、低比特数据通路、映射、HLS 与可重构系统 | [技术程序](https://www.isfpga.org/program/)；[历届入口](https://wp.isfpga.org/archive/) | [2026 CFP](https://isfpga.org/past/fpga2026/call-for-papers/) 明确覆盖架构、计算引擎、设计实例和 AI/ML；程序根路径可能随届更新 |
| **FCCM** | 定制计算架构、编程模型、工具、系统实现 | [2026 程序](https://www.fccm.org/fccm-26-program/) | [2026 CFP](https://www.fccm.org/call-for-papers-2026/)；同时关注架构和工具，区分主会论文与 demo / competition |
| **FPL** | 可重构架构与应用、设计方法、工具与 HLS | [会议系列入口](https://www.fpl.org/) → [2026 年度站点](https://2026.fpl.org/) 的 programme / proceedings | 系列站点用于找到当届页面；年度站点部分内容需要浏览器执行 JavaScript |
| **DAC** | 算法与硬件协同、加速器、设计自动化、系统优化 | [2026 程序](https://dac.com/2026/program) | [2026 Research Topics](https://dac.com/2026/research-topics)；从研究主题筛选，不需要浏览全部 EDA 或产业活动 |
| **ICCAD** | 硬件架构、映射与调度、设计自动化、软硬件协同 | [2026 年度站点](https://iccad.com/2026/) 的 Program；[2025 程序入口](https://2025.iccad.com/program-at-a-glance/) 可作回溯 | [2026 CFP](https://iccad.com/2026/authors/call-for-papers)；此处是 Computer-Aided Design 会议 |
| **DATE** | 系统级设计、设计工具、应用架构、嵌入式系统 | [2026 详细程序](https://date26.date-conference.com/programme) | [2026 CFP](https://date26.date-conference.com/call-for-papers)；[主题与 TPC](https://date26.date-conference.com/tpc)，按设计与应用等相关主题过滤 |
| **ASP-DAC** | 加速器设计、HLS、设计空间探索、系统级协同 | [2026 年度程序入口](https://www.aspdac.com/aspdac2026/)；[2026 程序 PDF](https://www.aspdac.com/aspdac2026/pdf/ASP-DAC_2026_Full_Program.pdf) | [2026 CFP](https://www.aspdac.com/aspdac2026/cfp/)；主会、设计竞赛与论坛分开记录 |

这些入口用于发现可以迁移的数据流、存储和实现方法。论文研究 CNN、ViT、LLM，甚至其他计算任务，也可能回答共同的资源复用或访存问题；应把原工作负载作为适用条件保留下来。

## 4. FPGA 架构主线关注的硬件期刊

| 期刊 | 阅读重点 | 官方范围与文章入口 |
|---|---|---|
| **ACM TRETS** — Transactions on Reconfigurable Technology and Systems | 可重构计算架构、工具链、系统与应用实现 | [ACM 期刊目录](https://dl.acm.org/journal/trets)；[编辑部范围页](https://trets.cse.sc.edu/tretscall.php) |
| **IEEE TCAD** — Transactions on Computer-Aided Design of Integrated Circuits and Systems | 综合、调度、映射、建模、设计空间探索与软硬件协同方法 | [IEEE CEDA 期刊页](https://ieee-ceda.org/publications/tcad)，含 scope、Current Issue 和 IEEE Xplore 入口 |
| **IEEE TVLSI** — Transactions on Very Large Scale Integration Systems | 硬件系统集成、逻辑与存储设计、性能资源权衡、可重构系统 | [IEEE CASS 期刊页](https://ieee-cas.org/publication/tvlsi)，含 scope 与 Current Issue 入口 |
| **IEEE TCAS-I** — Transactions on Circuits and Systems I: Regular Papers | 算术单元、电路与系统架构、算法到硬件的实现及分析 | [IEEE CASS 期刊页](https://ieee-cas.org/publication/TCAS-I)，从期刊链接进入 IEEE Xplore 文章列表 |

期刊按与量化或 FPGA 架构相关的关键词增量检查，不要求每期逐篇阅读。比较时保留 online / early access 日期、正式卷期年份和会议前身；扩展版与会议版关联为同一研究脉络。TRETS 编辑部范围页包含早期建刊表述，这里仅用于了解主题范围，不引用其时间性措辞。

## 5. 服务两条主线的补充来源

以下体系结构、系统与编译会议只按量化算法或 FPGA 架构的具体需要查阅；每次扩展检索先写明要解决的问题。

| 来源 | 什么时候值得扩展检索 | 官方程序实例 |
|---|---|---|
| **MICRO** | 执行机制、低比特计算与打包、存储层次、软硬件接口成为主要问题时 | [MICRO 2025 程序](https://microarch.org/micro58/program/index.php) |
| **HPCA** | 需要研究内存系统、带宽、异构计算和加速器架构时 | [HPCA 2026](https://2026.hpca-conf.org/)，含 Program 和论文入口 |
| **ISCA** | 需要比较通用架构机制、系统瓶颈和新计算组织时 | [ISCA 2026 程序](https://www.iscaconf.org/isca2026/program/) |
| **ASPLOS** | 需要理解架构、编译器、运行时和系统之间的共同优化时 | [ASPLOS 2026 详细程序](https://www.asplos-conference.org/asplos2026/program/index.html) |
| **PLDI** | 问题涉及硬件 DSL、程序表示、编译变换、可组合调度与正确性时 | [PLDI 2024 研究论文](https://pldi24.sigplan.org/track/pldi-2024-papers)，包含 Allo；后续使用对应年度研究论文页 |

优先检查能支持两条主线的 session、标题和摘要。来源不必使用 FPGA，但要说明对量化算法或 FPGA 架构的具体启发，以及哪些假设依赖 GPU/CPU/ASIC 的特定能力。

## 6. FPGA 架构的实现支撑：怎样阅读 HLS 资料

| 阅读对象 | 核心问题 | 应记录的证据 |
|---|---|---|
| **使用 HLS 实现加速器** | 作者如何把计算和数据流落到现有工具上 | 工具与版本、循环展开与流水化、数据通路、buffer / stream 组织、综合与实现报告、板上结果 |
| **提出 HLS / 编译方法** | 作者改进了怎样的硬件生成或优化机制 | 调度与绑定算法、存储推断或划分、DSL/IR、优化空间、正确性、跨设计评价与工具基线 |

同一论文可以同时涉及两类贡献，但“实现代码使用 HLS”本身不能证明它提出了新的 HLS 方法。本库将两类资料都放在 FPGA 架构的实现支撑下；只有当前设计需要某种调度、描述或编译能力时，才沿相关方法追踪到 PLDI 等来源。

## 7. 可复用的关键词组

先使用“问题词 + 机制词”，必要时加入模型或平台；不要在每个查询中强制加入 VLA。

| 检索组 | 关键词 | 组合示例 |
|---|---|---|
| 量化与误差 | `quantization`、`PTQ`、`QAT`、`low-bit`、`outlier`、`calibration`、`rotation`、`Hadamard`、`reconstruction`、`mixed precision` | `quantization rotation calibration`；`diffusion timestep quantization` |
| 数据表示与算术 | `integer-only`、`fixed-point`、`requantization`、`scale`、`packing`、`bit-serial`、`DSP`、`LUT` | `FPGA low-bit packing`；`integer attention requantization` |
| 数据流与存储 | `dataflow`、`pipeline`、`tiling`、`reuse`、`memory`、`bandwidth`、`buffer`、`streaming`、`roofline` | `transformer dataflow memory bandwidth`；`accelerator temporal spatial pipeline` |
| FPGA 实现支撑 | `high-level synthesis`、`HLS`、`scheduling`、`binding`、`memory banking`、`DSL`、`compiler`、`DSE`、`design space exploration` | `HLS scheduling binding`；`DSL composable accelerator`；`HLS memory design space exploration` |
| 应用标签 | `VLA`、`VLM`、`LLM`、`ViT`、`DiT`、`diffusion`、`flow matching`、`robot policy` | 只在需要定位结构、时间步或评测协议时加入这些词 |

读到可用机制后，沿参考文献和后续引用继续追踪。关键词没有命中的文章，也可以通过核心作者、开源项目和相关 session 发现。

## 8. 轻量维护节奏

以下是建议工作量，可随研究阶段调整。每周选题落在量化算法或 FPGA 架构之一，也可以比较两者的接口和相互影响。

| 时机 | 动作 | 留下的结果 |
|---|---|---|
| **每周增量检查，约 20–30 分钟** | 查看本周相关新稿、已跟踪论文更新和代码；快速筛选约 5–10 个标题 / 摘要 | 补充来源、问题标签和待读理由；不把“发现”写成“已读懂” |
| **每周选一个问题深入阅读** | 精读 1 篇主论文，并查 1–2 篇对照或前序工作 | 完成一份机制笔记；在横向表中新增或修正一行；记录一个仍需验证的问题 |
| **主会接收列表或论文集公开时** | 对当届相关 session 做集中扫描；标题筛选后再看摘要 | 当届候选清单，并记录扫描日期与已覆盖范围 |
| **每月一次短检查** | 查看重点期刊新增文章，修正发表状态、版本、链接和重复条目 | 保持参考信息与论文关系准确；调整下月优先阅读问题 |
| **准备轮到组会时** | 围绕一个具体问题组织主论文与横向对照 | 讲清机制、适用前提、与已有方法的差别，以及下一步可以验证的想法 |

没有新文章的会议不需要每周重扫整个网站。本文件不创建自动订阅或定时任务；若以后增加自动收集，抓取结果仍应先进入待筛选队列。

## 9. 文献来源与投稿候选分别判断

**文献来源**回答“在哪里能找到解决当前问题的方法”；**投稿候选**回答“最终贡献能否与某个研究社区的问题和证据要求对应”。采用 ICLR 的量化方法、参考 FPGA 会议的架构，均不自动决定最后的投稿场合。

| 最终工作的主要贡献 | 可以进一步考察的候选范围 | 需要先写清楚的内容 |
|---|---|---|
| 可迁移的量化算法、误差机制或校准方法 | ICLR、ICML、NeurIPS；视觉贡献对应 CVPR、ICCV、ECCV | 相比现有算法新增了什么机制，在哪些模型与精度条件成立 |
| 量化算法与 FPGA 架构共同优化 | FPGA / EDA 来源；根据实际贡献进一步考察 MLSys | 量化选择如何改变数据通路与访存，架构收益如何测量 |
| FPGA 数据流、计算与存储架构创新 | FPGA、FCCM、FPL；DAC、ICCAD、DATE、ASP-DAC；相关硬件期刊 | 架构原理、适用工作负载、性能与资源证据 |

这些是根据贡献选择阅读样本和考察对象的路线，不是录用预测。真正准备投稿时再核对当届 scope、track、格式和截止时间，本表不维护推测的时间表。

## 10. 预印本、正式论文与工程参考

在文献记录中明确区分资料类型：正式会议论文、期刊论文、workshop / short / demo、预印本，以及工程参考。发表状态和版本单独记录；同一工作的 arXiv 与正式版本相互关联，避免重复计数。

- **arXiv 等预印本**用于发现新方法。记录版本号与日期；没有官方接受信息时不填已录用的会议名称。
- **官方论文集与程序**用于核对题目、作者、场合和 track；程序页中出现的报告不一定对应主会长论文。
- **作者代码、竞赛方案、厂商文档**归为工程参考，可提供很好的实现思路。比如 [AICAS 2025 Track 2 第一名方案的中文 README](https://github.com/IEEE-AICAS/AICAS2025_GC/blob/main/Track2/1_sec/code/README.zh-CN.md) 可以用于研究 HLS 模块和数据流，不能仅凭竞赛名把 README 标成已发表主会论文。
- **访问受限时**保留官方元数据入口，再找作者公开稿；ACM / IEEE 全文是否可访问与论文是否已发表是两件事。年度站点失效时，回到会议系列网站和出版社论文集查找，并记录替代入口。

把资料纳入本库的标准是：它能帮助回答量化算法或 FPGA 架构中的一个明确问题，并且能说清所依赖的条件。应用标签保持开放，每周阅读围绕这两条主线积累比较与论证。
