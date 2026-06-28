<p align="center">
  <img src="https://raw.githubusercontent.com/kt-aicoding/.github/main/assets/logo.svg" alt="KT AI Coding logo" width="96" />
</p>

# KT AI Coding CLI Tools

面向 AI 编程环境的 CLI 工具集合，用来沉淀可复用的命令行工具、配置辅助、迁移脚本和本地开发环境治理经验。

这个仓库关注“命令行层”的确定性操作：配置、诊断、部署辅助、profile 切换、状态栏、脚本化迁移和工具健康检查。涉及 MCP server 的实现和策略放在 [`kt-aicoding/mcp-servers`](https://github.com/kt-aicoding/mcp-servers)，跨仓库索引放在 [`kt-aicoding/registry`](https://github.com/kt-aicoding/registry)。

## 边界

适合放在这里：

- 小型 AI 编程 CLI 工具。
- Claude Code / Codex / 其他 Agent CLI 的配置辅助工具。
- profile 切换、状态栏预设、本地初始化脚本和迁移工具。
- 云平台 CLI、GitHub CLI、本地工具链的使用规范和健康检查资料。
- 还没有必要独立成仓的实验性命令行工具。

不适合放在这里：

- MCP server 实现，放到 `kt-aicoding/mcp-servers`。
- 可复用 prompt/skill 指令，放到 `kt-aicoding/skills`。
- 单个成熟 CLI 产品，成熟后可以拆成独立仓库。
- API token、OAuth token、cookie、私有路径、私有项目上下文。

## 目录规划

```text
docs/
  local-cli-system.md
tools/
  <tool-name>/
```

| 路径 | 用途 |
| --- | --- |
| `docs/` | CLI 体系、治理原则、运维资料和操作规范 |
| `tools/` | 小型 CLI 工具或尚未独立成仓的工具原型 |

## 操作资料

| 文档 | 说明 |
| --- | --- |
| [文档索引](docs/README.zh-CN.md) | CLI 体系和操作资料入口。 |
| [本地 CLI 体系](docs/local-cli-system.md) | 当前 CLI 分层、平台登录态、仓库归属、维护命令和升级策略。 |

## 相关 Skill

| Skill | 用途 |
| --- | --- |
| [`cli-tooling-governance`](https://github.com/kt-aicoding/skills/blob/main/skills/codex/cli-tooling-governance/SKILL.md) | 判断 CLI、脚本、MCP、skill、独立仓库之间的边界，维护 CLI 成熟度、验证和升级策略。 |
| [`kt-aicoding-registry`](https://github.com/kt-aicoding/skills/blob/main/skills/codex/kt-aicoding-registry/SKILL.md) | 判断工具是否仍属于 `cli-tools`，还是应迁入其他 kt-aicoding 仓库或独立成仓。 |

## 如何把这个仓库当参考

这个仓库不只是放工具代码，也沉淀一套判断方式：

| 问题 | 判断标准 | 结果 |
| --- | --- | --- |
| 这是 CLI 还是 MCP？ | 需要稳定、可审计、可脚本化的人机命令行操作 | 放在 `cli-tools` |
| 是否该独立成仓？ | 有独立用户、独立发布节奏、独立安装文档 | 从 `tools/` 拆出独立仓库 |
| 是否该继续留在 docs？ | 只是工具治理、操作规范、维护策略 | 放在 `docs/` |
| 是否该写成 skill？ | 主要是 Agent 行为规范或任务流程 | 放到 `kt-aicoding/skills` |
| 是否该使用 provider MCP？ | CLI 已覆盖且更可审计时，不启用 MCP | 保持 CLI 优先 |

参考这个仓库时，优先看边界和操作资料，再看具体工具实现。

## 候选工具

- `ccuse` 迁移候选：Claude Code profile switcher。
- `codex-config-kit` 计划中：Codex 配置预设、状态栏预设和 setup helpers。

## 成熟度模型

| 阶段 | 形态 | 进入下一阶段的条件 |
| --- | --- | --- |
| 记录 | `docs/` 中的操作经验 | 有重复使用场景和明确命令序列 |
| 原型 | `tools/<tool-name>/` 中的小脚本 | 有 `--help`、错误处理和最小测试 |
| 工具 | 可安装、可版本化的 CLI | 有 README、示例、发布说明和稳定参数 |
| 独立项目 | 独立仓库 | 有独立用户、明确 roadmap 和维护边界 |

## 开发约定

- 每个工具应该能独立安装、运行和阅读文档。
- 优先提供 `--help`、`doctor` 或等价诊断入口。
- 涉及云平台时，先读资源状态，再做部署、删除、账单或域名操作。
- Provider 操作优先使用成熟 CLI，例如 `vercel`、`supabase`、`wrangler`、`tcb`、`netlify`、`flyctl`、`railway`、`gh`。
- 不默认启用 provider MCP 来替代成熟 CLI，除非任务明确需要 agent-native MCP 能力。

## 本地验证

这个仓库当前以文档和治理资料为主。提交前至少运行：

```bash
git diff --check
rg -n "gho_|Bearer|SERVICE_ROLE|password|cookie|secret|token|API_KEY|/Users/" README.md README.zh-CN.md docs || true
```

敏感扫描只允许命中安全说明或 placeholder，不允许真实凭据、私有路径或账号细节。
