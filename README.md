<p align="center">
  <img src="https://raw.githubusercontent.com/kt-aicoding/.github/main/assets/logo.svg" alt="KT AI Coding logo" width="96" />
</p>

# KT AI Coding CLI Tools

Command-line tools for configuring and operating AI coding environments.

中文说明见 [README.zh-CN.md](README.zh-CN.md)。

## Scope

This repository stages small CLI tools for AI coding workflows: profile switching, Codex configuration, status line presets, local setup helpers, and migration utilities.

Mature tools with independent users can graduate into standalone repositories.

## Layout

```text
docs/
  local-cli-system.md
tools/
  <tool-name>/
```

## Operating References

| Document | Notes |
| --- | --- |
| [Documentation Index](docs/README.md) | Entry point for CLI operating documents. |
| [Local CLI System](docs/local-cli-system.md) | Current CLI layers, provider login state, repository placement, maintenance commands, and upgrade policy. |

## Candidate Tools

- `ccuse` migration candidate: Claude Code profile switcher.
- `codex-config-kit` planned: Codex config presets, status line presets, and setup helpers.
