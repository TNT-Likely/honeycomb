# App 与 Cloud 隔离验收

提供 `isolated-app-cloud-qa` skill，用于移动 App 与自建 Cloud 的实服同步验收。

```sh
/plugin install app-cloud-qa@honeycomb
```

入口：[SKILL.md](skills/isolated-app-cloud-qa/SKILL.md)。执行脚本由各项目维护；本 plugin 保存隔离决策、验收流程、失败处理与报告要求。

BeeCount 的实现示例见 [项目适配说明](skills/isolated-app-cloud-qa/references/beecount.md)。这项能力不包含发版或合并 PR。
