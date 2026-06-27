<p align="center">
  <img src="https://raw.githubusercontent.com/kt-aicoding/.github/main/assets/logo.svg" alt="KT AI Coding logo" width="96" />
</p>

# KT AI Coding CLI Tools

用于配置和操作 AI 编程环境的命令行工具集合。

## 边界

这个仓库暂存小型 AI 编程 CLI 工具：profile 切换、Codex 配置、状态栏预设、本地初始化脚本和迁移工具。

成熟且有独立用户的工具可以拆分为独立仓库。

## 目录规划

```text
docs/
  local-cli-system.md
tools/
  <tool-name>/
```

## 操作资料

| 文档 | 说明 |
| --- | --- |
| [本地 CLI 体系](docs/local-cli-system.md) | 当前 CLI 分层、平台登录态、仓库归属、维护命令和升级策略。 |

## 候选工具

- `ccuse` 迁移候选：Claude Code profile switcher。
- `codex-config-kit` 计划中：Codex 配置预设、状态栏预设和 setup helpers。
