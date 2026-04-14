# first-deployment-check

本仓库仅包含一个 **Cursor Agent Skill**：在终端用 [Vercel 官方 CLI](https://vercel.com/docs/cli) 查看部署列表、成功/失败状态、单次部署详情与构建日志，减少对网页控制台来回切换的依赖。

## 内容说明

| 路径 | 说明 |
|------|------|
| `vercel-deployment-cli/SKILL.md` | Skill 正文（元数据 + 命令与工作流） |

## 本地使用（Cursor）

将 `vercel-deployment-cli` 目录复制到：

- **个人技能**：`~/.cursor/skills/vercel-deployment-cli/`
- **项目内技能**：`<你的项目>/.cursor/skills/vercel-deployment-cli/`

确保目录内存在 `SKILL.md`，重启或刷新 Cursor 后由描述自动匹配相关对话。

## 你需要自备的

1. 安装 CLI：`npm i -g vercel`（或 pnpm / 其他包管理器全局安装）。
2. `vercel login`。
3. 在应用根目录 `vercel link`（monorepo 可考虑 `vercel link --repo`）。

Skill 描述的是**部署发生之后**如何用命令行排查；**推送触发部署**来自 Vercel 与 Git 的集成，不是 Skill 本身执行部署。

## 许可证

如需开源发布，请自行补充 `LICENSE`（例如 MIT）。
