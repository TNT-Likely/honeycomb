# App 与 Cloud 隔离验收

提供 `isolated-app-cloud-qa` skill，用于移动 App 与自建 Cloud 的实服同步验收。

## Codex 本地安装

作为纯 skill 安装到 `~/.codex/skills/isolated-app-cloud-qa`，保留整个 skill 目录及 references、agents。使用 Codex 自带的 skill-installer 从本仓安装：

```sh
python3 "${CODEX_HOME:-$HOME/.codex}/skills/.system/skill-installer/scripts/install-skill-from-github.py" \
  --repo TNT-Likely/honeycomb \
  --path plugins/app-cloud-qa/skills/isolated-app-cloud-qa
```

安装后下一轮对话可使用 `$isolated-app-cloud-qa`，也可按场景自动选用。更新时从同一仓库同步 skill 目录；不要把正文复制进项目 AGENTS.md。

## Claude Code 插件安装

```sh
/plugin install app-cloud-qa@honeycomb
```

入口：[SKILL.md](skills/isolated-app-cloud-qa/SKILL.md)。执行脚本由各项目维护；本 plugin 保存隔离决策、验收流程、失败处理与报告要求。

BeeCount 的实现示例见 [项目适配说明](skills/isolated-app-cloud-qa/references/beecount.md)。这项能力不包含发版或合并 PR。

自动验收后交接正常 App 与已登录的 Cloud 网页，保持隔离服务运行，等用户明确验收完成后再关闭。完整结果以独立离线 HTML / ZIP 交付，不进入功能分支。
