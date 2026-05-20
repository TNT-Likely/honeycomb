# 🍯 honeycomb

> sunxiao 的 Claude Code 能力集 —— skills、agents、commands、hooks 的蜂巢式集合。

每个蜂房(plugin)装一组相关能力,可以独立安装、独立升级、独立卸载。

## 安装

```bash
# 1. 把这个 marketplace 加进你的 Claude Code
/plugin marketplace add TNT-Likely/honeycomb

# 2. 装具体的 plugin
/plugin install fastapi-vite-saas@honeycomb
```

> 需要 Claude Code 支持 plugin marketplace 功能。如果命令不识别,请升级到最新版 Claude Code。

## 现有 Plugins

| Plugin | 简介 | 状态 |
|---|---|---|
| [fastapi-vite-saas](./plugins/fastapi-vite-saas/) | FastAPI + pnpm/Vite + Docker + GHA 单仓双端项目脚手架,基于 BeeCount-Cloud 生产实践 | 🟢 v0.1.0 |

(更多 plugin 陆续加入中)

## 设计哲学

**沉淀决策,不沉淀代码。**

传统 template repo 把"代码骨架"冻在某个时刻,新项目 clone 下来要做一轮考古:依赖版本旧了、CI 脚本过时、命名 placeholder 散落各处。AI 时代不需要这种考古 —— 把"为什么这么选"和"这么做的理由"写成 skill,让 AI 在生成时携带上下文判断、按你新项目的语义改造。

每个 plugin 的 SKILL.md 因此都包含:

- **决策表**(每一项选型的理由)
- **反模式**(已踩过的具体坑,带原因)
- **变体扩展**(切 Postgres、切 Vue、加 MCP 等知识点)
- **执行流程**(参数 → 生成 → 自检 → 后续提示)

让 AI 不只是机械模板替换,而是带着架构判断生成代码。

## 给别的 plugin 作者:贡献新蜂房

如果你想往 honeycomb 加一个 plugin:

```
plugins/<your-plugin>/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   └── <skill-name>/
│       └── SKILL.md
└── README.md
```

然后在根 `.claude-plugin/marketplace.json` 的 `plugins[]` 数组里追加一条。提 PR 即可。

## 开发与本地测试

本地把 honeycomb 仓拉下来后,可以直接 `/plugin marketplace add <绝对路径>` 装本地版,改了 SKILL.md 就 reload。

```bash
git clone https://github.com/TNT-Likely/honeycomb.git
# 在 Claude Code 里:
/plugin marketplace add /path/to/honeycomb
/plugin install fastapi-vite-saas@honeycomb
```

## License

MIT © sunxiao
