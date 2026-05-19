# bee-stack

脚手架 Claude Code 插件:基于 [BeeCount-Cloud](https://github.com/TNT-Likely/BeeCount-Cloud) 的生产实践,一键生成 **FastAPI + pnpm/Vite 前端 + Docker + GitHub Actions** 的单仓双端项目。

## 这个插件能做什么

跟 Claude 说一句"按 bee-stack 起一个新项目叫 Foo",它会:

1. 问清几个关键参数(项目名、前端框架、数据库、可选模块)
2. 生成完整目录树和所有基础文件(`src/`、`frontend/`、Dockerfile、Makefile、GHA workflows、Alembic migrations、.env.example、双语 README ...)
3. `git init` → `make setup-backend` → `make migrate` → `make test` → `pnpm build` 跑一遍自检
4. 给出"还需要你手动做的 5 件事"清单(改 JWT_SECRET、push GitHub、配 Docker Hub secrets ...)

## 安装

```bash
/plugin marketplace add TNT-Likely/honeycomb
/plugin install bee-stack@honeycomb
```

## 适用场景

适合:个人 / 小团队的自托管 SaaS、内部工具、副业产品。一个 docker run 就能跑起来的那种。

不适合:纯前端项目(用 `pnpm create vite` 就够了)、纯 CLI 工具、库项目、需要多服务编排的微服务架构。

## 技术栈

| 层 | 选型 |
|---|------|
| Backend | FastAPI + SQLAlchemy 2.x + Alembic + Pydantic v2 |
| Auth | JWT(PyJWT)+ passlib + 可选 TOTP 2FA |
| Frontend | pnpm workspace + Vite + React/Vue/Svelte 任选 |
| Infra | 多阶段 Dockerfile(Node builder → python:3.12-slim) |
| CI/CD | GitHub Actions(ci.yml + release.yml) |
| 编排 | Makefile(setup-backend / migrate / dev-api / dev-web / test / lint / typecheck / wipe-local) |

## 设计原则

- **单镜像可跑**:`docker run` 一行起服务,挂一个 `/data` volume 就能全量备份
- **统一入口**:本地和生产都用 `uvicorn server:app`,不分裂
- **按需启用 add-on**:备份、2FA、MCP、AI 文档 Q&A 这些都是可选,默认 scaffold 是最小核心
- **Makefile 是唯一编排入口**:不引入 just/task,所有人都会读 Make

## 反模式提示

skill 文档里专门列了"BeeCount-Cloud 有但新项目不要默认抄"的部分(同步架构、RAG 索引、域特定导入逻辑),以及踩过的具体坑(HEALTHCHECK 路径、Dockerfile layer cache、`server.py` 只能一行等)。

## 详情

完整 skill 内容见 [`skills/scaffold/SKILL.md`](./skills/scaffold/SKILL.md)。
