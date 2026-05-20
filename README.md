# 🍯 honeycomb

> sunxiao 的 **AI 开发能力沉淀仓** —— 跨工具可复用的 skills 集合,当前以 Claude Code plugin 形式分发。

每个蜂房(plugin)装一组相关能力,Claude Code 用户一键安装、独立升级、独立卸载;其他工具用户直接读取每个 plugin 里的 `SKILL.md`(纯 markdown + YAML frontmatter,工具无关)。

## 安装

```bash
# 1. 把这个 marketplace 加进你的 Claude Code
/plugin marketplace add TNT-Likely/honeycomb

# 2. 装具体的 plugin(按需选)
/plugin install fastapi-vite-saas@honeycomb
/plugin install flutter-riverpod-drift@honeycomb
```

> 需要 Claude Code 支持 plugin marketplace 功能。如果命令不识别,请升级到最新版 Claude Code。

## 现有 Plugins

| Plugin | 简介 | 状态 |
|---|---|---|
| [fastapi-vite-saas](./plugins/fastapi-vite-saas/) | FastAPI + pnpm/Vite + Docker + GHA 单仓双端项目脚手架,基于 BeeCount-Cloud 生产实践 | 🟢 v0.1.0 |
| [flutter-riverpod-drift](./plugins/flutter-riverpod-drift/) | Flutter + Riverpod + Drift app 脚手架,带 Design Token + Repository 多端切换,基于 BeeCount 生产实践 | 🟢 v0.1.0 |

(更多 plugin 陆续加入中)

## 在其他 AI 工具里用 honeycomb

`/plugin marketplace add` 是 Claude Code 独占机制,但每个 plugin 里 `skills/<x>/SKILL.md` 是**纯 markdown + YAML frontmatter**,工具无关 — 任何 LLM agent 都能消化:

| 工具 | 怎么用 |
|---|---|
| **Claude Code** | `/plugin marketplace add TNT-Likely/honeycomb` 一键装(原生) |
| **OpenAI Codex CLI** | SKILL.md 格式与 Claude Code 兼容,把内容塞进项目根 `AGENTS.md` 或 `--system` 参数 |
| **Cursor** | `cp plugins/<x>/skills/<y>/SKILL.md .cursor/rules/<name>.mdc`(逐项目),或用社区 [rule-porter](https://github.com/nedcodes-ok/rule-porter) 自动转换 |
| **Cline / Continue.dev** | 复制 SKILL.md 到 VSCode 设置的 custom instructions / `~/.continue/config.json` |
| **Aider** | `cat plugins/<x>/skills/<y>/SKILL.md >> CONVENTIONS.md` |
| **其他** | 直接把 SKILL.md 内容粘到对话开头当 system prompt |

**Ground truth**:SKILL.md 内容工具无关,只有分发机制是 Claude Code 独占。社区目前没有跨工具统一的 skill 分发协议(MCP 管"工具调用",不覆盖 skills)。

## 设计哲学

**沉淀决策,不沉淀代码。**

传统 template repo 把"代码骨架"冻在某个时刻,新项目 clone 下来要做一轮考古:依赖版本旧了、CI 脚本过时、命名 placeholder 散落各处。AI 时代不需要这种考古 —— 把"为什么这么选"和"这么做的理由"写成 skill,让 AI 在生成时携带上下文判断、按你新项目的语义改造。

每个 plugin 的 SKILL.md 因此都包含:

- **决策表**(每一项选型的理由)
- **反模式**(已踩过的具体坑,带原因)
- **变体扩展**(切 Postgres、切 Vue、加 MCP 等知识点)
- **执行流程**(参数 → 生成 → 自检 → 后续提示)

让 AI 不只是机械模板替换,而是带着架构判断生成代码。

## 什么进 honeycomb,什么独立开仓?

honeycomb 定位是**个人能力沉淀仓**,不是"造爆款 skill"的平台。判定准则:

> 这个 skill 单独拎出来,值不值得有自己的 landing page、npm 包、独立 star 计数?

| 类型 | 例子 | 去哪 |
|---|---|---|
| 个人脚手架 / 反模式 checklist / 私货 lint 规则 / 团队约定 | `fastapi-vite-saas`、`flutter-riverpod-drift` | 进 honeycomb marketplace |
| 有 viral 潜质、清晰受众群、值得独立营销的 flagship skill | 行业级"通用代码审查 skill"、专项审计工具等 | 单独开仓(参考 [ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) 模式 — 单 skill + 自带 marketplace 壳 + CLI 跨工具 installer),honeycomb 里只列链接 |

为什么这么拆:个人 utility skill 一起放方便维护(同作者风格 / 同受众 / 同 git);flagship skill 单独开仓才能独立刷品牌、独立计 star、独立投流 — marketplace 模式把信号混在一起,反而稀释爆款。

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
