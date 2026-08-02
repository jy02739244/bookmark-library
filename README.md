# Bookmark Library

独立维护的公共书签库，供 iori-nav 后台「公共书签库导入」使用。

## 目录结构

```
bookmark-library/
├── .githooks/
│   └── pre-commit          # 自动更新 index.json 的 count 和 updated
├── index.json              # 清单：列出所有可用书签库
├── README.md
└── libs/
    ├── ai-tools.json       # AI 工具 (10)
    ├── dev-tools.json      # 开发工具 (4)
    ├── forum.json          # 论坛 (3)
    └── temp-mail.json      # 临时邮箱 (5)
```

## 文件格式

每个书签文件采用 iori-nav 的系统导出格式，即 `{ "category": [...], "sites": [...] }`：

- `category`：分类列表，字段含 `id`、`catelog`、`parent_id`、`sort_order`
- `sites`：书签列表，字段含 `name`、`url`、`catelog_id`、`sort_order`、`logo`、`desc`

`logo` 为空时，iori-nav 导入会自动通过 `ICON_API` 生成 favicon，无需手动填写。

> 公共书签库均为公开书签，无需添加 `is_private` 字段；导入时默认视为公开。

## 如何使用

1. 在新目录 `libs/` 下新建一个书签 JSON 文件，例如 `libs/music.json`。
2. 在 `index.json` 的 `libraries` 数组中登记一条，给出 `id`、`name`、`description`、`file` 和 `count`。
3. 推送仓库后，在 iori-nav 后台即可看到并通过导入流程使用。

`count` 请与文件内书签数量保持一致，方便后台展示。

### 自动更新 count

项目配置了 git pre-commit 钩子，提交前会自动扫描各 library 文件，更新 `index.json` 中的 `count` 和 `updated` 日期。克隆仓库后首次使用时，执行一次以下命令启用钩子：

```bash
git config core.hooksPath .githooks
```

设置后每次 `git commit` 都会自动处理 count，无需手动修改。

## 版本管理

每次修改书签内容后，请打一个新的 Git tag 发布版本：

```bash
git tag v1.0.0
git push origin v1.0.0
```

iori-nav 后台通过 jsDelivr CDN 加载书签库，URL 中的 `@v1.0.0` 引用 tag 名称。更新书签库后只需打新 tag、更新 iori-nav 中 `PUBLIC_LIBRARY_SOURCE.ref` 的版本号即可立即生效，无需等待 CDN 缓存过期。
