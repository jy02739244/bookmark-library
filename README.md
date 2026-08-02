# Bookmark Library

独立维护的公共书签库，供 iori-nav 后台「公共书签库导入」使用。

## 目录结构

```
bookmark-library/
├── index.json        # 清单：列出所有可用书签库
├── README.md
└── libs/
    ├── ai-tools.json # AI 工具
    ├── dev-tools.json
    └── news.json
```

## 文件格式

每个书签文件采用 iori-nav 的系统导出格式，即 `{ "category": [...], "sites": [...] }`：

- `category`：分类列表，字段含 `id`、`catelog`、`parent_id`、`sort_order`、`is_private`
- `sites`：书签列表，字段含 `name`、`url`、`catelog_id`、`sort_order`、`is_private`、`logo`、`desc`

`logo` 为空时，iori-nav 导入会自动通过 `ICON_API` 生成 favicon，无需手动填写。

> 公共书签库均为公开书签，无需添加 `is_private` 字段；导入时默认视为公开。

## 如何使用

1. 在新目录 `libs/` 下新建一个书签 JSON 文件，例如 `libs/music.json`。
2. 在 `index.json` 的 `libraries` 数组中登记一条，给出 `id`、`name`、`description`、`file` 和 `count`。
3. 推送仓库后，在 iori-nav 后台即可看到并通过导入流程使用。

`count` 请与文件内书签数量保持一致，方便后台展示。
