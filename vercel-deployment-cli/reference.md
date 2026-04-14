# Vercel CLI 部署排查 — 参考

本文件补充 [SKILL.md](SKILL.md) 中未展开的链接与排障细节；按需阅读。

## 项目链接

- **`.vercel/project.json`**：`vercel link` 单项目。
- **`.vercel/repo.json`**：`vercel link --repo`，适用于 monorepo 或多目录多项目。

命令应在包含 `.vercel` 的目录（或其子目录）执行，避免选错项目。子应用目录（如 `apps/web`）通常比仓库根更明确。

## 常见错误

- **选错团队**：`vercel whoami`，必要时切换团队后重新 `vercel link`。
- **未 link 仍想查列表**：使用 Dashboard 中的 **项目名** 作为 `vercel list <project-name>`（与仓库名可能一致，以控制台为准）。
- **`vercel logs` 提示需 link**：在已 link 目录执行，或按 `vercel help logs` 使用 `--project` 等参数。

## 帮助命令

```bash
vercel help list
vercel help inspect
vercel help logs
```
