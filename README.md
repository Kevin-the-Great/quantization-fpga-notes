# Reading Notes on Quantization Algorithms and FPGA Architectures

[English](README.md) | [中文](README.zh-CN.md)

An ongoing reading collection with two tracks: **quantization algorithms** and **FPGA architectures**. VLA is the current application focus; methods may come from LLMs, ViTs, CNNs, diffusion models, or other applications with relevant computation or memory patterns.

The quantization track covers low-bit representations, quantization error, and calibration. The FPGA architecture track covers computation, memory, and dataflow organization, with HLS and mapping tools supporting implementation. Papers from other fields are included with a clear explanation of which problem they help address in either track.

**Organize by technical problem, discover through conferences and journals, and compare under explicit conditions.** Each entry should explain the problem it solves, its assumptions, its relevance to the current research, and what still needs to be validated.

Updated: 2026-09-16. **26 papers and 1 engineering reference**, including **13 papers/preprints from 2026**. DiTPA (ISCA 2026) is the current preferred seminar candidate. Screening notes are assistant-prepared; personal reading, reproduction, and presentation progress are recorded separately.

Dates below identify a verified public version: arXiv v1, publisher online date for UDP, or the institution-reported publication date for DiTPA. They do not guarantee the earliest disclosure. **2026 includes accepted/published 2026 papers and 2026 preprints**; LUT-LLM first appeared in November 2025. Accepted means the official program was verified; published means a proceedings source was verified. See the [catalog and date fields](data/README.md).

**Next seminar priority: [DiTPA — ISCA 2026](notes/ditpa-2026.md).** Focus on how action-planning redundancy informs accelerator design. [Preparation plan](weekly/2026-09-16.md) · [Talk outline](talks/ditpa-2026.md). Earlier candidates remain in the [reading plan](weekly/2026-09-14.md).

[Quantization Algorithms](#quantization-algorithms) · [FPGA Architectures](#fpga-architectures) · [Conferences and Journals](venues.md) · [Reading Notes](notes/hgpipe-2024.md) · [Weekly Notes](weekly/2026-09-16.md) · [Workflow](WORKFLOW.md)

Detailed notes and supporting documents are currently in Chinese.

## Quantization Algorithms

Compare reconstruction, scaling, rotation, and timestep calibration while preserving the original models, numerical formats, and applicability conditions. See the [quantization comparison](comparisons/quantization.md) for the full analysis.

### 2026 Papers

| Public version date | Venue / status | Paper | Mechanism and comparison focus |
|---|---|---|---|
| 2026-07-02 | arXiv · preprint | [OrbitQuant](https://arxiv.org/abs/2607.02461) | Normalized rotations and shared nonuniform codebooks; calibration-free ranges still require online operations. [Note](notes/orbitquant-2026.md) |
| 2026-05-27 | arXiv · preprint | [HoloQ-VLA](https://arxiv.org/abs/2605.28803) | Composite rotations and per-step scales for W4A4 language and diffusion action modules; v3 updated on August 11. [Note](notes/holoq-vla-2026.md) |
| 2026-05-03 | FCCM · accepted | [ViM-Q](https://arxiv.org/abs/2605.01935) | APoT weights, token-wise activation quantization, and lookup/SSM pipelines; power is estimated. [Note](notes/vim-q-2026.md) |
| 2026-04-24 | FCCM · accepted | [HGQ-LUT](https://arxiv.org/abs/2604.22293) | Train quantized logic-lookup layers with a resource objective; hardware evidence is OOC post-route. [Note](notes/hgq-lut-2026.md) |
| 2026-04-13 | ICML · accepted | [ReSpinQuant](https://arxiv.org/abs/2604.11080) | Layer-wise rotations and low-rank residual alignment; compare accuracy gains with online correction costs. [Note](notes/respinquant-2026.md) |
| 2026-03-19 | arXiv · preprint | [6Bit-Diffusion](https://arxiv.org/abs/2603.18742) | NVFP4/INT8 activation routing plus temporal caching; isolate precision selection from skipped computation. [Note](notes/6bit-diffusion-2026.md) |
| 2026-02-23 | CVPR · published | [QuantVLA](https://openaccess.thecvf.com/content/CVPR2026/html/Zhang_QuantVLA_Scale-Calibrated_Post-Training_Quantization_for_Vision-Language-Action_Models_CVPR_2026_paper.html) | Attention temperature and output-energy calibration for selective W4A8; diffusion attention projections retain floating point. [Note](notes/quantvla-2026.md) |
| 2026-02-03 | ICLR · published | [QVLA](https://proceedings.iclr.cc/paper_files/paper/2026/hash/fa064215307efaad75bebc7a2e3194a4-Abstract-Conference.html) | Action-sensitive channel bit allocation; weight bits are an average budget, with projector and action head kept in BF16. [Note](notes/qvla-2026.md) |
| 2026-01-14 | ACL · published | [MXFP PTQ Benchmark](https://aclanthology.org/2026.acl-long.1854/) | PTQ under MXFP formats; shared-scale error and module sensitivity. MXFP4 differs from INT4. [Note](notes/mxfp-ptq-benchmark-2026.md) |
| 2025-11-09 | FCCM · accepted | [LUT-LLM](https://arxiv.org/html/2511.06174v2) | Vector co-quantization and memory lookups; model conversion and training required. Preprint began in 2025. [Note](notes/lut-llm-2026.md) |

### 2025 H2 Supplement

| Public version date | Venue / status | Paper | Mechanism and comparison focus |
|---|---|---|---|
| 2025-11-10 | ICCAD · published | [QUARK](https://arxiv.org/html/2511.06767v1) | Share nonlinear sub-operators through quantization and approximation; compare reuse with concurrent execution. [Note](notes/quark-2025.md) |
| 2025-09-30 | WASPAA · accepted | [Audio DiT PTQ](https://arxiv.org/abs/2510.00313) | Timestep smoothing and FP16 low-rank compensation; unresolved A4/A8 table labels. [Note](notes/audio-dit-ptq-2025.md) |

### Earlier Context

| Year | Venue | Paper | Mechanism and comparison focus |
|---|---|---|---|
| 2025 | ICLR | [SpinQuant](https://proceedings.iclr.cc/paper_files/paper/2025/hash/e5b1c0d4866f72393c522c8a00eed4eb-Abstract-Conference.html) | Learned orthogonal rotations; optimization cost, transformations that can be folded into parameters, and online transformations |
| 2024 | NeurIPS | [QuaRot](https://proceedings.neurips.cc/paper_files/paper/2024/hash/b5b939436789f76f08b9d0da5e81af7c-Abstract-Conference.html) | Rotations to mitigate outliers; distinguish low-bit representations from actual operator precision |
| 2024 | NeurIPS | [PTQ4DiT](https://proceedings.neurips.cc/paper_files/paper/2024/hash/72d32f4fe0b7af03732bd227bf1c4a5f-Abstract-Conference.html) | Channel salience balancing and calibration across timesteps for DiTs |
| 2024 | MLSys | [AWQ](https://proceedings.mlsys.org/paper_files/paper/2024/hash/42a452cbafa9dd64e9ba4aa95cc1ef21-Abstract-Conference.html) | Activation-aware weight quantization; scaling and its implementation in inference kernels |
| 2023 | ICLR | [GPTQ](https://arxiv.org/abs/2210.17323) | Weight reconstruction and error compensation using second-order information |
| 2023 | ICML | [SmoothQuant](https://proceedings.mlr.press/v202/xiao23c.html) | Equivalent channel scaling; the quantization difficulty of activations versus weights, and static versus dynamic scales |
| 2023 | ICCV | [Q-Diffusion](https://arxiv.org/abs/2302.04304) | Timestep calibration and branch quantization; iterative error and structural assumptions |

## FPGA Architectures

Compare compute arrays, dataflows, hardware reuse, pipelines, and memory organization, with HLS and mapping tools as implementation support. See the [FPGA architecture comparison](comparisons/architectures.md) for the full analysis.

### 2026 Papers

| Public version date | Venue / status | Paper | Mechanism and comparison focus |
|---|---|---|---|
| 2026-08-04 | ISCA · published | [DiTPA](https://doi.org/10.1109/ISCA66397.2026.00188) | Action prediction, denoising reuse, and multimodal scheduling. Transfer reference; public artifact uses simulation, hardware platform pending full-text verification. [Note](notes/ditpa-2026.md) |
| 2026-05-03 | FCCM · accepted | [ViM-Q](https://arxiv.org/abs/2605.01935) | APoT weights, token-wise activation quantization, and lookup/SSM pipelines; power is estimated. [Note](notes/vim-q-2026.md) |
| 2026-04-24 | FCCM · accepted | [HGQ-LUT](https://arxiv.org/abs/2604.22293) | Train quantized logic-lookup layers with a resource objective; hardware evidence is OOC post-route. [Note](notes/hgq-lut-2026.md) |
| 2026-04-23 | FCCM · accepted | [GraphLeap](https://arxiv.org/abs/2604.21290) | Overlap graph construction and feature updates; dependency changes require fine-tuning. [Note](notes/graphleap-2026.md) |
| 2026-02-21 | FPGA · published | [UDP](https://doi.org/10.1145/3748173.3779194) | Parameterized DSP packing and signed corrections; verify useful products for each operand width. [Note](notes/udp-2026.md) |
| 2025-11-09 | FCCM · accepted | [LUT-LLM](https://arxiv.org/html/2511.06174v2) | Vector co-quantization and memory lookups; model conversion and training required. Preprint began in 2025. [Note](notes/lut-llm-2026.md) |

### 2025 H2 Supplement

| Public version date | Venue / status | Paper | Mechanism and comparison focus |
|---|---|---|---|
| 2025-11-10 | ICCAD · published | [QUARK](https://arxiv.org/html/2511.06767v1) | Share nonlinear sub-operators through quantization and approximation; compare reuse with concurrent execution. [Note](notes/quark-2025.md) |
| 2025-07-04 | ICCAD · published | [Hummingbird](https://arxiv.org/html/2507.03308v2) | DSP reuse, aligned DDR access, and GQA buffering; W4 storage differs from INT24 vector computation. [Note](notes/hummingbird-2025.md) |

### Earlier Context

| Year | Venue | Paper | Mechanism and comparison focus |
|---|---|---|---|
| 2024 | ICCAD | [HG-PIPE](https://arxiv.org/html/2407.17879v1) | A hybrid-grained pipeline for ViTs; global dependencies, buffering, and parallelism. [Reading note](notes/hgpipe-2024.md) |
| 2024 | FPGA | [FlightLLM](https://arxiv.org/abs/2401.03868) | Compute and memory mapping for LLMs; the interaction of low precision, sparsity, and the memory hierarchy |
| 2024 | PLDI / PACMPL | [Allo](https://www.csl.cornell.edu/~zhiruz/pdfs/allo-pldi2024.pdf) | HLS support reference: composable hardware customization and mapping across multiple compute kernels |

### Engineering Reference

| Year | Source | Implementation | Reading focus |
|---|---|---|---|
| 2025 | AICAS Grand Challenge · Track 2 | [EdgeDecoding / 1_sec](https://github.com/IEEE-AICAS/AICAS2025_GC/blob/main/Track2/1_sec/code/README.zh-CN.md) | HLS matrix kernels, separate accumulators, attention modules, and memory interfaces; recorded as an engineering resource |

## What to Compare

| Track | Methods and assumptions | Conditions to record alongside results |
|---|---|---|
| Quantization algorithms | Reconstruction objectives, calibration data, scaling and rotation, bit widths and exceptions, additional online operations | Model, quantization scope, evaluation protocol, task metrics |
| FPGA architectures | Compute reuse, pipelines, on-chip buffers, external memory communication, low-bit datapaths | Platform, frequency, workload, precision, timing boundaries, synthesis or measurement evidence |

Record numerical results in the [results table](data/measurements.csv) by experimental configuration. Preserve the workload and timing boundaries associated with each paper's images/s, tokens/s, or policy response latency.

## Start Here

| Resource | Purpose |
|---|---|
| [Paper catalog](data/papers.csv) | Main catalog readable in GitHub, Excel, or scripts: sources, publication venues, technical tags, and reading status |
| [Conferences and journals](venues.md) | Sources to follow, official entry points, and search keywords |
| [Quantization comparison](comparisons/quantization.md) | Reconstruction, scaling, rotation, timestep calibration, and deployment conditions |
| [FPGA architecture comparison](comparisons/architectures.md) | Hardware reuse, pipelines, and memory; relevant HLS tools as implementation references |
| [HG-PIPE example reading note](notes/hgpipe-2024.md) | How to break a paper down into questions, evidence, insights, and hypotheses to validate |
| [2026 seminar reading plan](weekly/2026-09-14.md) | Recent main papers, targeted comparisons, and presentation questions |
| [Example FPGA architecture presentation](talks/transformer-reuse-and-pipeline.md) | A presentation built around an architecture comparison; quantization can also be presented independently |
| [Workflow](WORKFLOW.md) | How to add resources, record progress, and update comparisons each week |
| [Data fields](data/README.md) | How to record paper metadata and experimental configurations |

## Tracks and Technical Tags

| Track | Scope |
|---|---|
| **Quantization algorithms** · `quantization` | Numerical representations, weight reconstruction, error compensation, outliers, rotation, scaling, calibration, mixed precision, and timestep-related quantization |
| **FPGA architectures** · `fpga-architecture` | Compute arrays, low-bit datapaths, hardware reuse, pipelines, on-chip storage, external memory and communication; relevant HLS, mapping, scheduling, and design space exploration |

The catalog's `tracks` field records the research track; work spanning both can carry both labels. The `topics` field records specific techniques such as rotation, caching, pipelines, and HLS. Model families and platforms such as FPGA/GPU/ASIC describe application or implementation conditions. Evaluation methods support the comparison of evidence in both tracks.

## Questions for the First Reading Cycle

1. **Quantization algorithms:** What do reconstruction, scaling, and rotation each change? How do statistics across timesteps affect calibration? Are additional costs incurred offline or during inference?
2. **FPGA architectures:** What do hardware reuse and spatial pipelines each save and consume? How do bit width, model size, and memory access conditions affect the choice? How can HLS and mapping methods help implement it?

## Minimum Requirements for a New Entry

- Add a stable ID, title, publication source, year, technical tags, and reason for inclusion to the catalog.
- After screening, decide whether to read in depth, retain as background, or defer. Previously read resources can also be added.
- During an in-depth read, complete a [reading card](templates/paper-note.md) and add or revise a finding in the relevant comparison table.
- Numerical claims must be traceable to a table or configuration in the source. Results with different hardware, precision, workloads, or timing boundaries should not be combined into a speed ranking.
- Keep open questions explicit: identify the evidence to check or experiments to run where uncertainty remains.

## Organization References

The classification and paper index draw on [Awesome Learning-based Odometry](https://github.com/KwanWaiPang/Awesome-Learning-based-VO-VIO); the comparison fields draw on [Neural Network Accelerator Comparison](https://nicsefc.ee.tsinghua.edu.cn/projects/neural-network-accelerator.html). The [paper catalog](data/papers.csv) holds the complete metadata, while technical assessments are continually revised in the comparison tables and reading notes.
