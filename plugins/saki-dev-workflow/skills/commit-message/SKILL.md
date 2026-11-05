---
name: commit-message
description: 根据改动生成规范的 Git 提交信息。
---

# 提交信息

使用 Conventional Commits：`type(scope): 摘要`

- type：feat / fix / docs / refactor / test / chore / perf
- 摘要不超过 50 字，使用祈使语气，不以句号结尾
- 正文说明“为什么改”而不是“改了什么”，每行不超过 72 字
- 破坏性改动在正文加 `BREAKING CHANGE:`

先查看 diff 再写，不要编造没有发生的改动。
