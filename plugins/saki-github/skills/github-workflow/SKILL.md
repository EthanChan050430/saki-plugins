---
name: github-workflow
description: 通过 GitHub 工具查阅仓库、Issue 与 Pull Request，并安全地进行写操作。
---

# GitHub 工作流

- 查询类操作（搜索仓库、读取文件、列出 Issue/PR）可以直接执行。
- 创建或评论 Issue/PR、推送文件等写操作，先向用户确认仓库、标题与内容再执行。
- 引用 Issue/PR 时使用 `owner/repo#编号` 格式。
- 如果工具提示认证失败，告诉用户需要设置 `GITHUB_PERSONAL_ACCESS_TOKEN` 环境变量后重启面板，不要索要令牌明文。
