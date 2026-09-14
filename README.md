# Reading Notes on Quantization Algorithms and FPGA Architectures

[English](README.md) | [中文](README.zh-CN.md)

An ongoing reading collection with two tracks: **quantization algorithms** and **FPGA architectures**. VLA is the current application focus; methods may come from LLMs, ViTs, CNNs, diffusion models, or other applications with relevant computation or memory patterns.

The quantization track covers low-bit representations, quantization error, and calibration. The FPGA architecture track covers computation, memory, and dataflow organization, with HLS and mapping tools supporting implementation. Papers from other fields are included with a clear explanation of which problem they help address in either track.

**Organize by technical problem, discover through conferences and journals, and compare under explicit conditions.** Each entry should explain the problem it solves, its assumptions, its relevance to the current research, and what still needs to be validated.

Updated: 2026-09-14. The initial collection contains **10 papers and 1 engineering reference**, with further additions planned. Initial screening notes were prepared with assistant support; reading, reproduction, and presentation progress are recorded separately.

[Quantization Algorithms](#quantization-algorithms) · [FPGA Architectures](#fpga-architectures) · [Conferences and Journals](venues.md) · [Reading Notes](notes/hgpipe-2024.md) · [Weekly Notes](weekly/2026-09-14.md) · [Workflow](WORKFLOW.md)

Detailed notes and supporting documents are currently in Chinese.

## Quantization Algorithms

Compare reconstruction, scaling, rotation, and timestep calibration while preserving the original models, numerical formats, and applicability conditions. See the [quantization comparison](comparisons/quantization.md) for the full analysis.

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
| [First reading cycle](weekly/2026-09-14.md) | Alternate between the two tracks, adjusting for prior reading |
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
