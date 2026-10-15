# S0：环境与 Skills 审计清单

## 目的

在创建实际应用骨架前，确认本机工具版本、Skills 真实目录、入口和依赖，避免重复安装或重写现有能力。

## A. 本机环境（只读检查）

在 Mac 终端逐条运行：

```bash
sw_vers
git --version
node --version
npm --version
python3 --version
```

记录命令输出。若某个命令不存在，只记录错误，不要为了通过检查而立即安装。

## B. Skills 目录清单

```bash
ls -la "/Users/zaihu.zhou/.workbuddy/skills/"
ls -la "/Users/zaihu.zhou/.workbuddy/skills/hu-inv-dd"
ls -la "/Users/zaihu.zhou/.workbuddy/skills/hu-inv-fin-analysis"
ls -la "/Users/zaihu.zhou/.workbuddy/skills/pe-vc-analyst"
```

如果某条路径不存在，不要据此推断技能已丢失；先查看目录清单中的真实名称。

## C. 每个 Skill 要记录的项目

- 实际目录名称
- 主要文件（README、SKILL.md、pyproject.toml、requirements.txt 或 package.json 等）
- 运行入口与命令
- 语言和依赖
- 输入参数与输出格式
- 是否调用外部模型/API
- 凭证如何提供（只记录方式，不复制凭证）
- 能否单独运行、已知错误和限制

## D. 安全提醒

不要将 `.env` 内容、API 密钥、访问令牌、密码或客户/项目敏感资料发到对话中。分享目录结构时可只发文件名与命令输出。

## E. 完成标准

- [ ] 记录操作系统与工具版本
- [ ] 确认实际 Skills 名称和目录
- [ ] 至少识别每个 Skill 的入口文档
- [ ] 明确哪些 Skill 可复用、哪些需要适配、哪些待验证
- [ ] 根据结果确认前端/后端方案与运行方式
- [ ] 再进入 S1：建立应用骨架和最小工作台