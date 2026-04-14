---
name: vercel-deployment-cli
description: >-
  Guides checking Vercel deployments from the terminal using the official CLI:
  list deployments and status, inspect a deployment, read build logs and runtime
  logs. Use when the user asks about deployment status, failed builds, stuck
  deploys, production showing old code, avoiding the dashboard, post-push checks,
  cron or CI verification, or running vercel list, vercel inspect, or vercel logs.
---

# Vercel 终端部署排查（CLI）

## Quick start

1. 在含 `.vercel` 的目录执行（或先 `vercel link` / `vercel link --repo`）；不确定团队时运行 `vercel whoami`。
2. `vercel list <project-name> --yes`（已 link 可省略项目名）→ 看最新一条 **Ready / Error**。
3. 若 **Error**：取该行的 deployment URL → `vercel inspect <url> --logs`，根据构建日志修复。
4. 若关心「线上是否最新」：对比最新 **Ready** 与刚推送的提交/时间；能访问但非最新通常表示最新构建失败、别名仍指向上一次 Ready。
5. 部署已成功但需要查运行时问题：`vercel logs <deployment-url>`。

## 目标

在终端完成 **列表 → 详情 → 日志**，减少反复打开 Dashboard；输出可配合脚本（`--format json`）或 Agent 自动执行上述步骤。

## 重要说明

- **推送后自动构建**来自 Vercel 与 Git 的集成（或手动 `vercel deploy`），不是本 skill「触发部署」；本 skill 只覆盖**部署发生后的查验与排障**。
- **域名能打开 ≠ 最新代码**：最新一条若是 Error，生产仍可能绑定上一版 Ready。

## 前置条件

1. 已安装：`npm i -g vercel`（或 pnpm / 其他方式全局安装）。
2. `vercel login`。
3. `vercel link`（monorepo 多应用见 [reference.md](reference.md)）。
4. `vercel` 不在 PATH 时：将 `$(npm config get prefix)/bin` 加入 PATH。

## 核心命令

### 列出部署

```bash
vercel list <project-name> --yes
```

列表顶部一般为最新。筛选与 JSON：

```bash
vercel list <project-name> --status ERROR --yes
vercel list <project-name> --status READY --yes
vercel list <project-name> --format json --yes
```

### 单次部署详情（可不依赖本地 link）

```bash
vercel inspect https://www.example.com
vercel inspect https://your-app-xxxx.vercel.app
```

### 构建日志（失败时）

```bash
vercel inspect <deployment-url> --logs
```

构建错误（如 `next build`）多在日志末尾。

### 运行时日志（已成功部署）

```bash
vercel logs <deployment-url>
```

## Agent 检查清单

- [ ] 已定位含 `.vercel` 的工作目录或显式传入项目参数。
- [ ] 已运行 `vercel list`（必要时 `--status ERROR`）。
- [ ] 失败时用 `vercel inspect <url> --logs` 拉构建日志，再改代码或配置。
- [ ] 「旧版线上」场景：核对最新 Ready 与 Git 提交是否一致。

## 定时 / 推送后

在 cron、CI 或对话中复用 **Quick start**；JSON 输出便于脚本解析。

## Additional resources

- 链接方式、monorepo、常见踩坑：[reference.md](reference.md)
- 官方 CLI：<https://vercel.com/docs/cli>
