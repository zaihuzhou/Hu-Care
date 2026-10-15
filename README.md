# 祜·Care（Hu-Care）

AI-native 个人 PE 投资经理工作台。

## 当前阶段

当前处于 **S0：产品与工程准备**。先固定产品和 UI 基准、审计本地 Skills 与开发环境，再开始应用骨架开发。当前仓库中的文档属于初始基线，技术选型仍需通过环境和 Skills 审计确认。

## 产品目标

围绕个人 PE 投资经理的项目研究流程，逐步建立：
- AI 投资 Copilot 统一入口
- 投资研究引擎：尽调、财务分析、风险核查、证据和报告
- 投资认知引擎：投资判断、假设跟踪和复盘
- 共享能力：项目、来源/证据、任务、Skills、连接器和用户可管理的记忆

## 已确认约束

- UI 以 `docs/UI_BASELINE.md` 中记录的用户指定 Hu-Fin 工作台截图为基准。
- 本地 Skills 路径：`/Users/zaihu.zhou/.workbuddy/skills/`
- PE 分析 Skill 名称以 `pe-vc-analyst` 为准，不再假设存在 `hu-inv-PE`。
- 不把候选数据源视为已接通；先核实授权、API 和运行条件。
- AI 研究结论需尽可能关联来源，并清楚区分事实、推断、假设和未知。

## 文档导航

- [产品需求](docs/PRD.md)
- [技术架构](docs/ARCHITECTURE.md)
- [数据模型](docs/DATA_MODEL.md)
- [UI 基准](docs/UI_BASELINE.md)
- [开发计划](docs/DEVELOPMENT_PLAN.md)
- [开发环境与技术栈](docs/ENVIRONMENT_AND_STACK.md)

## 开发原则

1. 先完成 S0 审计，再决定具体依赖和目录。
2. 复用可运行的 Skills，不为追求架构形式而重写。
3. 每阶段按验收标准推进。
4. 不提交密钥、令牌、密码或未经授权的第三方材料。

## 下一步

按 `docs/ENVIRONMENT_AND_STACK.md` 运行本机环境检查，并核实 Skills 目录结构；将结果用于确定最终技术栈和创建应用骨架。
