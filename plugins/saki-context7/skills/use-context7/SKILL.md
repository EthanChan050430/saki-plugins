---
name: use-context7
description: 写代码前用 Context7 查询库的最新文档，避免使用过时的 API。
---

# 使用 Context7 查询最新文档

当用户要求使用某个第三方库/框架编写或修改代码时：

1. 先调用 Context7 的 `resolve-library-id` 工具，把库名解析成 Context7 库 ID。
2. 再调用 `get-library-docs`（或同名工具），带上与任务相关的主题关键词，获取最新文档。
3. 以文档为准编写代码，不要凭记忆使用可能已废弃的 API。
4. 回答时简要注明所参考的库版本与文档来源。

如果 Context7 工具不可用或查询失败，说明原因后再基于已知知识回答，并提示可能存在版本差异。
