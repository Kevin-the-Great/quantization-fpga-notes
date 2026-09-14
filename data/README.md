# 数据字段

## papers.csv：一项工作一行

标签之间用分号分隔。字段内的逗号由标准 CSV 引号处理。

| 字段 | 含义 |
|---|---|
| `id` | 稳定 ID；笔记、比较表与结果表均用它关联 |
| `title` | 正式题名或工程资源名称 |
| `year` | 正式发表年份；工程资源使用对应版本/活动年份 |
| `venue` | 正式场合；预印本填 arXiv 等来源；工程资源明确写工程来源 |
| `resource_type` | `paper` / `preprint` / `engineering` |
| `publication_status` | `published` / `preprint` / `unconfirmed` / `not_applicable` |
| `tracks` | 主线：`quantization` / `fpga-architecture`；交叉工作可用分号同时关联两条 |
| `topics` | 旋转、校准、流水线、存储、HLS 等技术标签，可多选；归属于上述主线 |
| `workloads` | 原文涉及的模型或任务类型，可多选 |
| `why_read` | 为什么收录；明确可研究或迁移的问题 |
| `source_url` | 正文、正式论文页或工程资源主入口 |
| `venue_evidence_url` | 核实发表场合的出处；工程资源可与主入口相同 |
| `reading_status` | 个人阅读进度，见 WORKFLOW.md |
| `presented_on` | 实际汇报日期，未登记则留空 |
| `assistant_review` | 助理整理范围，与个人阅读进度分开；初始 `source_screened`，示例笔记为 `draft_note` |
| `source_checked_on` | 本次来源核对日期，不表示穷尽后续版本 |
| `note_path` | 相对于库根目录的笔记路径；尚无独立笔记可留空 |

## measurements.csv：一个实验配置一行

初始只有表头。当前两张横向比较表是方法与条件的定性初筛；数值将在精读对应实验后逐项补入。

| 字段组 | 字段 | 要记录什么 |
|---|---|---|
| 身份 | `measurement_id`, `paper_id`, `configuration` | 配置 ID、关联论文、原文实验配置名称 |
| 数值与范围 | `weight_bits`, `activation_bits`, `quantization_scope`, `precision_exceptions`, `scaling` | 权重/激活位宽、量化覆盖、例外层/算子、缩放粒度与静态/动态方式 |
| 工作负载 | `model`, `benchmark`, `protocol` | 模型及规模、任务、batch/序列长度/步数/种子等相关条件 |
| 实现条件 | `platform`, `implementation`, `frequency_mhz` | 芯片或板卡、实现方式、频率；不适用时写 `NA` |
| 证据 | `evidence_level` | `numerical_simulation` / `hls_estimate` / `synthesis` / `post_route` / `device_measured` 等 |
| 指标 | `metric`, `value`, `unit`, `timing_boundary`, `baseline` | 指标及单位、数值、计时范围、比较基线；不同指标可分行 |
| 溯源 | `source_url`, `source_locator`, `checked_on`, `notes` | 公开来源、页码/表格/配置行、核对日期与限制 |

原文未报告的必需信息写 `NR`，不适用写 `NA`，尚未查清写 `TBD`。模型精度与硬件性能属于不同指标；需要在同一配置下确认它们是否对应。

作者报告的实测数据仍是**作者的测量**；个人复现须另行记录实现、配置与结果。峰值吞吐和端到端实测时延应作为不同记录，保留各自边界。
