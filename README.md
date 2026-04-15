# vercel-deployment-check

本仓库提供一个 **Cursor Agent Skill**：在终端用 [Vercel 官方 CLI](https://vercel.com/docs/cli) 查看部署列表、成功/失败状态、单次部署详情与构建/运行时日志，减少反复打开 Vercel 网页控制台。

## 仓库里有什么

| 路径 | 说明 |
|------|------|
| [`vercel-deployment-cli/SKILL.md`](vercel-deployment-cli/SKILL.md) | 技能主文件（YAML 元数据 + 命令与工作流） |
| [`vercel-deployment-cli/reference.md`](vercel-deployment-cli/reference.md) | 可选补充（链接方式、monorepo、常见踩坑） |

## 怎么用（Cursor）

1. 把 **`vercel-deployment-cli` 整个文件夹**复制到下面其一：
   - **个人技能**（所有项目可用）：`~/.cursor/skills/vercel-deployment-cli/`
   - **仅当前项目**：`<你的项目根>/.cursor/skills/vercel-deployment-cli/`
2. 确保目录里有 `SKILL.md`。
3. 重启 Cursor 或重新加载窗口后，对话里提到部署状态、Vercel 失败、构建日志等时，Agent 可按技能描述自动匹配。

## 你需要先准备好的（与 Skill 无关，是 CLI 本身）

1. 安装 CLI：`npm i -g vercel`（或 pnpm / 其他方式全局安装）。
2. `vercel login`。
3. 在要查的应用目录执行 `vercel link`（monorepo 可考虑 `vercel link --repo`）。

说明：推代码后自动构建来自 **Vercel 与 Git 的集成**；本 Skill 教的是构建/部署发生之后，**如何在终端排查**，不是替你执行 `git push` 或触发部署。

## 许可证

如需开源发布，请自行添加 `LICENSE`（例如 MIT）。
