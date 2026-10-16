# 祜·Care 数据模型基线

- 状态：V0.1 / 概念模型
- 说明：以下为领域草案，不是已执行的数据库迁移

## 1. 建模原则

- 项目、来源、证据、任务、判断是核心实体。
- 把原始来源与 AI 生成的解释分开存储。
- 重要结论要能追踪到证据和任务执行版本。
- 允许记录“未知、待验证、冲突证据”，不强迫所有问题得出确定结论。
- 记忆写入应可查看、修改和删除；不要默认把所有对话永久写入长期记忆。

## 2. 核心实体

| 实体 | 用途 | 关键字段（概念） |
|---|---|---|
| Project | 投资项目 | id, name, company, stage, status, thesis, created_at, updated_at |
| Source | 信息来源 | id, project_id, source_type, title, uri, publisher, published_at, retrieved_at, access_note |
| Material | 项目材料 | id, project_id, source_id, filename, mime_type, storage_key, checksum, uploaded_at |
| Evidence | 证据/事实摘录 | id, project_id, source_id, material_id, excerpt, claim, evidence_type, confidence, as_of_date |
| ResearchTask | 研究任务 | id, project_id, task_type, status, requested_by, input_json, created_at, started_at, finished_at |
| TaskRun | 单次执行 | id, task_id, skill_id, skill_version, model_id, status, output_ref, error_code, started_at, finished_at |
| Skill | 技能注册信息 | id, name, version, runtime, input_schema, output_schema, enabled |
| Finding | 研究发现 | id, project_id, task_run_id, title, description, finding_type, confidence, review_status |
| FindingEvidence | 发现与证据关联 | finding_id, evidence_id, relationship, note |
| Risk | 风险事项 | id, project_id, title, severity, likelihood, rationale, mitigation, status |
| InvestmentJudgment | 投资判断 | id, project_id, decision, thesis, counter_thesis, owner_note, decided_at, review_status |
| Assumption | 待验证假设 | id, project_id, statement, test_method, status, due_date, result |
| Review | 决策复盘 | id, project_id, judgment_id, outcome, lessons, reviewed_at |
| MemoryItem | 用户可管理的记忆 | id, scope, content, source_ref, created_at, updated_at, retention_status |
| WorkflowRun | 工作流实例 | id, project_id, template_id, status, current_step, started_at, finished_at |
| AuditEvent | 审计事件 | id, actor, action, target_type, target_id, occurred_at, metadata |

## 3. 关系概览

- 一个 Project 可以拥有多个 Source、Material、Evidence、ResearchTask、Finding、Risk 和 InvestmentJudgment。
- Source 可以来自文件、网页、数据库连接器或用户手工录入。
- Material 可关联 Source；Evidence 可引用 Source 和/或 Material。
- ResearchTask 可有多个 TaskRun，用于记录重试、版本变化和不同阶段执行。
- Finding 通过 FindingEvidence 关联一个或多个 Evidence。
- InvestmentJudgment 可关联 Assumption 与 Review。
- WorkflowRun 组织多个任务步骤；每一步的状态与人工确认应可追踪。
- MemoryItem 的作用域必须明确，避免项目资料与通用个人偏好混淆。

## 4. 证据类型建议

- `source_fact`：来源直接支持的事实或摘录
- `calculation`：基于明确输入计算得出的结果
- `model_inference`：模型推断，必须标记为推断
- `assumption`：尚待验证的假设
- `contradiction`：与其他证据存在冲突
- `unknown`：目前无法确认的信息

置信度应与来源质量和验证状态配合使用，不能用一个分数掩盖证据缺失。

## 5. 状态字段建议

- ResearchTask / TaskRun：queued, running, waiting_for_review, succeeded, failed, cancelled
- Finding：draft, needs_evidence, reviewed, accepted, rejected
- Risk：open, monitoring, mitigated, closed
- Assumption：untested, testing, supported, rejected, inconclusive

具体枚举在实现前统一，避免前后端各自定义。

## 6. 数据治理与隐私

- 不在数据库中保存不必要的账号密码、API 密钥或完整认证令牌。
- 来源 URI、访问时间、发布/数据日期应尽可能保留。
- 对敏感材料限制访问，下载和导出应有明确路径。
- 提供删除项目及其关联材料/记忆的策略；明确备份中的保留边界。
- 向量索引若启用，必须与源数据的权限和删除策略一致。

## 7. 仍待确认

- 项目字段是否需要覆盖基金、交易结构、估值、退出路径等 PE 专属信息。
- 材料存储采用本地目录还是对象存储。
- 证据摘录是否需要页码、段落、表格单元格等定位信息。
- 记忆的默认保留策略与用户控制方式。
\n\n## 2026-10 修订：判断版本与数据生命周期\n除 Project、Task、SourceMaterial、Evidence、Finding、Analysis、Conclusion 外，补充/明确 UserJudgment、JudgmentRevision、ReviewRecord、ConnectorStatus 与 AuditEvent 等概念。该清单是领域模型建议，不代表数据库迁移已实施。\n\nEvidence 尽可能记录来源类型、文件/来源标识、获取时间、原文位置、核验状态、核验人及关联结论。必须区分原始材料、外部数据、AI 生成内容、用户确认判断和个人认知资产。\n\n重要判断不应被简单覆盖：保留版本、当时证据、假设、反证、用户修改原因和后续结果。复盘要区分当时可用信息与后来新增证据，避免结果偏差。明确各类数据的归属、访问、保留、导出和删除范围；删除时检查材料、证据、报告、索引和缓存等关联数据。缺失、冲突、过期或未授权数据应显式标记，不得猜测填补。