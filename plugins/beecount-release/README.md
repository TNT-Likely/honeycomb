# 已迁移到 BeeCount 项目

`beecount-release` 与 BeeCount 的代码、发布和验收配置绑定，已迁回 [BeeCount/.agents/skills/beecount-release](https://github.com/TNT-Likely/BeeCount/tree/main/.agents/skills/beecount-release)。本路径仅保留迁移说明，不再分发 plugin 或全局 skill。

Codex 使用项目 `.agents/skills`，Claude Code 使用项目 `.claude/skills` 引用同一源码。Cloud / Website 需要时使用项目安装器，具体步骤见 [项目 skill 安装说明](https://github.com/TNT-Likely/BeeCount/blob/main/docs/contributing/PROJECT_SKILLS_ZH.md)。

曾安装旧 plugin 时先安装项目入口并核对，再卸载 `beecount-release@honeycomb`；旧用户级副本可通过 BeeCount 的 `migrate-global` 命令备份并移出全局发现目录。不要继续使用旧 plugin 安装命令。
