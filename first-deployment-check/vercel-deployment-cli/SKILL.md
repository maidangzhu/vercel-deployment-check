---
name: vercel-deployment-cli
description: >-
  Guides checking Vercel deployments from the terminal with the official CLI
  (list status, inspect deployment, build/runtime logs). Use when the user asks
  about deployment status, failed builds, production still showing an old
  version, avoiding the Vercel dashboard, post-push verification, scheduled
  checks, or commands like vercel list, vercel inspect, or vercel logs.
---

# Vercel 终端部署排查（CLI）

## 目标

减少反复打开 Vercel 网页控制台确认「是否部署成功 / 是否还是旧版」的成本；在终端完成**列表 → 详情 → 构建日志**闭环，便于 AI 或脚本自动化跟进。

## 重要说明（避免误解）

- **推代码自动部署**来自 Vercel 与 Git 仓库的集成（或 CLI 手动 `vercel deploy`），**不是**本 skill 触发的魔法；本 skill 教的是部署发生后如何用 CLI **查验状态与日志**。
- **线上能打开但代码旧**：常见于最新一次构建 **Error**，生产域名仍指向**上一次 Ready** 的部署；必须用 `vercel list` / `inspect` 确认，不能只看域名可访问。

## 前置条件

1. 已安装 CLI：`npm i -g vercel`（或 `pnpm add -g vercel` 等等价方式）。
2. 已登录：`vercel login`。
3. 项目已关联：在应用根目录（monorepo 则为对应子包目录）执行 `vercel link`；多项目仓库可用 `vercel link --repo`。
4. 在含 `.vercel` 的目录下执行命令，或显式传项目参数（见 `vercel help list`）。

若 `vercel` 找不到，将 `$(npm config get prefix)/bin` 加入 `PATH`。

## 核心命令

### 1. 列出部署（Ready / Error / Building）

```bash
vercel list <project-name> --yes
```

列表**最上方一般为最新部署**。Status 如 `● Ready`、`● Error`。仅 Error 时，生产仍可能指向上一版 Ready。

```bash
vercel list <project-name> --status ERROR --yes
vercel list <project-name> --status READY --yes
vercel list <project-name> --format json --yes
```

已 `link` 的目录可直接 `vercel list`（省略项目名）。

### 2. 单次部署详情（可不依赖本地 link）

已知生产别名或预览 URL：

```bash
vercel inspect https://www.example.com
vercel inspect https://your-app-xxxx.vercel.app
```

输出含 status、deployment id、时间、别名等。

### 3. 构建日志（定位失败原因）

对失败行上的 **deployment URL**：

```bash
vercel inspect <deployment-url> --logs
```

构建阶段错误（如 `next build`）多在日志**末尾**。

### 4. 运行时日志（部署已成功）

```bash
vercel logs <deployment-url>
```

若提示需 link，在已 link 目录执行，或按 CLI 说明使用 `--project` 等参数。

## 给 Agent 的推荐流程

1. 在仓库中定位已 link 目录（含 `.vercel`），必要时 `vercel whoami` 确认团队。
2. 执行 `vercel list ... --yes`（或带 `--status ERROR`）看最新状态。
3. 若 Error：对该条 URL 执行 `vercel inspect <url> --logs`，根据构建日志改代码或配置。
4. 若用户关心「线上是否新」：对比最新 Ready 的 **Git 提交 / 时间**与预期推送是否一致。

## 与定时任务 / 推送后检查

- **定时**：cron / CI / 个人自动化里调用上述命令，解析 `--format json`。
- **推送后**：在 hook 或 CI 步骤里跑 `vercel list`，或由用户在会话里说「刚推了，帮看下 Vercel」，Agent 按上面流程执行。

## 参考

- `vercel help list`、`vercel help inspect`、`vercel help logs`
- 官方文档：<https://vercel.com/docs/cli>
