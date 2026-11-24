# 日常数字任务耗时审计与 AI 批处理实证

> 使用同一批10个文件的批量重命名任务，对比纯人工与AI直接批量处理方式的耗时和命名正确性。

## 1. 项目简介

- **A组：纯人工**——实验者按统一规则自行判断并重命名。
- **B组：AI直接批量处理**——实验者向AI提供10个原始文件并提交Prompt，AI直接完成文件重命名并提供可下载结果；实验者下载处理后的文件，不逐个手动重命名。
- 两组均处理同一批10个原始文件。AI处理的是实验者上传的文件副本，不是直接操作实验者本地电脑。

统一格式：`日期_类别_主题.扩展名`。类别只能是：`作业、笔记、截图、资料`。

## 2. 实验结果

### 耗时

| 组别 | 第1次 | 第2次 | 第3次 | 平均耗时 |
|---|---:|---:|---:|---:|
| 纯人工 A组 | 516秒 | 442.20秒 | 360.28秒 | 439.49秒 |
| AI直接批量处理 B组 | 289秒 | 196秒 | 122秒 | 202.33秒 |

- A组总耗时：1318.48秒，平均439.49秒（约7分19秒）。
- B组总耗时：607秒，平均202.33秒（约3分22秒）。
- 平均耗时差：237.16秒。
- B组平均耗时比A组低约 **53.96%**。

A2原始计时照片显示00:07:22.20（442.20秒），A3原始计时照片显示00:06:00.28（360.28秒）；此前A2数据表中的422秒为录入错误，已按原始照片更正。人工组从开始人工处理任务计时，到10个文件全部完成重命名时停止；AI组从提交Prompt开始计时，到AI处理后的文件下载完成时停止。两组最终正确性检查均不计入耗时。AI组计时包含AI处理和结果下载，不等同于纯模型生成时间。B组第1次（B1）计时照片显示2026-10-24、00:04:49.64（289.64秒），统计表保留原实验记录的整数秒289秒；计时照片精确显示289.64秒，存在0.64秒精度差，未用照片读数替换原始统计记录；B组第2次（B2）正确计时照片显示2026-10-29、00:03:16.42（196.42秒），统计表保留原实验记录的整数秒196秒；计时照片精确显示196.42秒，存在0.42秒精度差，未用照片读数替换原始统计记录。B组第3次（2026-11-08）对应122秒。B1、B2日期与轮次的对应关系现已由用户提供的计时照片确认；统计表仍按原始整数秒口径记录289秒和196秒，精确读数保留在证据说明中。

### 文件名正确性

| 组别 | 逐轮结果 | 正确数/总数 | 准确率 | 核验依据 |
|---|---|---:|---:|---|
| A组纯人工 | A1：9/10；A2：10/10；A3：9/10 | 28/30 | 93.33% | GitHub中A1、A2、A3最终结果文件名逐项审核 |
| B组AI直接批量处理 | B1、B2、B3均为0/10 | 0/30 | 0% | 三轮原始ZIP逐项审计 |
| 合计 |  | 28/60 | 46.67% | 两组均为逐项文件名核验，但证据载体不同 |

**A组专项审核：**
- A1：9/10。错误文件名为 `2026-10-22_截图-课程.png`，应使用下划线分隔字段。审核记录见 `evidence/manual/A1/A1_output_audit.csv`。
- A2：10/10。计时原始照片显示442.20秒，已用于更新耗时统计。审核记录见 `evidence/manual/A2/A2_output_audit.csv`。
- A3：9/10。计时原始照片显示360.28秒，已用于更新耗时统计。错误文件名为 `2026-10-18_作业_英语.pdf`，对应原始文件为英语资料，预期类别应为“资料”。审核记录见 `evidence/manual/A3/A3_output_audit.csv`。

A组本次统计是对GitHub目录中30个最终结果文件名按既定映射进行的事后审核。它不是对文件内部内容、实验全过程或原始计时材料的独立验证。错误文件名均按提交时状态保留，没有为了提高正确率而修改。

**B组专项复核：** B1（2026-10-24）、B2（2026-10-29）和B3（2026-11-08）的原始ZIP均已逐项核验。每轮10个文件均不符合本实验命名规则，因此三轮均为0/10。逐项结果和证据清单分别见 `evidence/ai/B1/`、`evidence/ai/B2/`、`evidence/ai/B3/`。B3逐项审计CSV的实际仓库路径为 `data/ai_trial3_output_audit.csv`。

此前“A组30/30正确，100%”和“B组20/30正确”等旧统计均已撤回，当前以A组28/30、B组0/30为准。

## 3. 解释与局限

本实验中AI直接批量处理并交付下载文件的平均耗时较短，但三轮AI处理结果均未满足严格命名规则。A组事后文件名审核正确率为93.33%，B组原始ZIP审计正确率为0%。因此不能声称AI直接处理在保持命名质量的同时节省时间。

准确结论是：**本次小样本中，AI直接处理组平均耗时降低约53.96%，但命名正确率为0%；人工组命名正确率为93.33%，平均耗时较长。** 这说明在本次规则和样本下，未经规则校验的AI批量重命名结果存在严重风险。

每组仅3次，且同一实验者重复操作，存在熟练度效应。A组审核依据是GitHub中最终结果文件名及预定映射，未下载并逐一验证文件内容；A组A1、A2、A3均有实验过程视频，用户确认视频覆盖从开始操作到10个文件完成；这些视频不是屏幕录制。A2、A3另有计时照片，A1没有独立计时照片，因此A1的516秒主要依据现存实验记录。B组正确性有三轮原始ZIP逐项审计支持。B组视频提取帧是派生参考材料，不是原始静态截图。B组第1、2次的日期与耗时映射已由用户提供的计时照片确认：B1为2026-10-24、00:04:49.64；B2为2026-10-29、00:03:16.42。

## 4. 仓库目录

```text
.
├── README.md
├── data/
│   ├── experiment_data.csv
│   ├── filename_mapping.csv
│   └── ai_trial3_output_audit.csv
├── docs/
│   ├── experiment_design.md
│   ├── timing_method.md
│   ├── ai_prompt.md
│   ├── repository_audit.md
│   ├── analysis/
│   │   └── prompt_failure_analysis.md
│   └── verification/
│       ├── validation_rules.md
│       ├── validation_execution_record.md
│       └── ai_error_cases.md
├── evidence/
│   ├── manual/
│   │   ├── A1/
│   │   ├── A2/
│   │   └── A3/
│   └── ai/
│       ├── B1/
│       ├── B2/
│       └── B3/
└── reports/
    └── experiment_analysis.md
```

## 5. 交付状态

- [x] 记录A组、B组各3次耗时并复算均值
- [x] 复算耗时降低比例
- [x] 审核A组A1、A2、A3共30个最终结果文件名
- [x] 逐项审计B组第1、2、3轮原始ZIP，共30个文件
- [x] 归档A组逐轮文件名审核CSV和清单
- [x] 归档B组第1、2、3轮原始ZIP与屏幕录制证据包
- [x] 保存B组各轮审计CSV、文字摘录、证据清单和SHA-256
- [x] 同步README、实验设计、数据表、分析报告和审查记录
- [x] 登记A组A1、A2、A3过程视频；用户确认视频覆盖完整操作过程
- [ ] 补充A1独立计时照片（目前缺失，不补造）
- [x] 根据计时照片确认B组第1、2轮耗时与日期的对应关系
- [x] 建立人机校验规则与执行清单（`docs/verification/validation_rules.md`）
- [x] 建立A组与AI组真实错误案例库（`docs/verification/ai_error_cases.md`）

## 6. 交付文件导航

- 实验设计与操作流程：[`docs/experiment_design.md`](docs/experiment_design.md)
- 计时口径：[`docs/timing_method.md`](docs/timing_method.md)
- Prompt失败原因分析（含B1–B3已找回任务指令句对照）：[`docs/analysis/prompt_failure_analysis.md`](docs/analysis/prompt_failure_analysis.md)
- B组原始Prompt归档核查：[`docs/analysis/prompt_archive_status.md`](docs/analysis/prompt_archive_status.md)
- AI Prompt 与执行流程说明：[`docs/ai_prompt.md`](docs/ai_prompt.md)（区分现存模板与逐轮原始Prompt证据）
- 原始实验数据与标准映射：[`data/experiment_data.csv`](data/experiment_data.csv)、[`data/filename_mapping.csv`](data/filename_mapping.csv)
- 实验分析报告：[`reports/experiment_analysis.md`](reports/experiment_analysis.md)
- 人工组A1–A3过程视频、计时照片与审核记录索引：[`evidence/manual/README.md`](evidence/manual/README.md)
- 仓库审查记录：[`docs/repository_audit.md`](docs/repository_audit.md)

## 7. 人机校验交付

- 校验规则：[`docs/verification/validation_rules.md`](docs/verification/validation_rules.md)
- 历史结果回顾性校验记录：[`docs/verification/validation_execution_record.md`](docs/verification/validation_execution_record.md)
- 真实错误案例：[`docs/verification/ai_error_cases.md`](docs/verification/ai_error_cases.md)
- 校验规则区分机器可执行检查与人工语义复核，并明确原始错误结果不得覆盖。
- 错误案例按文件计数：A组2个错误文件，B组30个错误文件，共32个；同一文件违反多条规则仍只计为一个错误文件。
- 规则文档是交付时制定的复核规范，不代表这些规则已在原始六轮实验中全部执行。

## 8. 数据诚信说明

耗时与正确性是不同指标。A组最终结果文件名审核为28/30；B组原始ZIP审计为0/30。A组的审核范围是GitHub最终结果文件名与标准映射，不包括文件内容及全过程；B组则以原始ZIP内部文件名为主要判定依据。两组均保留错误结果，不在事后修改原始实验输出。未取得的证据不标记为已完成。
