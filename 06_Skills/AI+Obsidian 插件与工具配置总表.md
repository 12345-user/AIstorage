# AI+Obsidian 插件与工具配置总表

更新时间：2026-06-08

## 当前已启用

| 类型 | 名称 | 状态 | 当前功能 | 保存/作用位置 |
| --- | --- | --- | --- | --- |
| Obsidian 社区插件 | Claudian / realclaudian | 已安装并启用 | 在 Vault 内连接 Codex/Claude 等命令行助手，允许 AI 读写、搜索、整理整个仓库 | `.obsidian/plugins/realclaudian` |
| Obsidian 核心插件 | Daily Notes | 已启用 | 生成每日简报 | `05_Review/每日简报` |
| Obsidian 核心插件 | Templates | 已启用 | 插入资料、知识卡、选题、输出、周报、脚本、实验报告模板 | `_templates` |
| Obsidian 核心插件 | Markdown Importer | 已启用 | 导入旧笔记、Markdown/部分外部笔记格式 | `01_Sources/旧资料导入` |
| Obsidian 核心插件 | Slash Command | 已启用 | 用 `/` 快速调用命令 | 全 Vault |
| Obsidian 核心插件 | Graph / Backlinks / Outgoing Links | 已启用 | 查看双链、出链、知识图谱 | 全 Vault |
| Codex 插件 | Browser | 当前 Codex 可用 | 打开/测试本地网页、检查 localhost 或文件页面 | Codex 工作区 |
| Codex 插件 | Documents | 当前 Codex 可用 | 创建、编辑、渲染校验 Word 文档 | Codex 工作区 |
| Codex 插件 | Presentations | 当前 Codex 可用 | 创建、编辑、渲染校验 PPTX | Codex 工作区 |
| Codex 插件 | Spreadsheets | 当前 Codex 可用 | 创建、分析、格式化表格 | Codex 工作区 |

## 图片蓝图中提到但本地未安装

| 插件/工具 | 目标功能 | 计划保存位置 | 当前缺口 |
| --- | --- | --- | --- |
| GitHub 定时抓取项目 Horizon | 定时抓取 GitHub Trending、行业新闻、RSS、论文报告 | `00_Inbox/自动抓取`、`05_Review/每日简报` | 需要项目安装、关键词、频率、GitHub/API 权限 |
| Web Clipper / Clipper | 网页文章、视频字幕、灵感内容剪藏 | `00_Inbox/手动剪藏` | 需要安装浏览器扩展或 Obsidian 剪藏插件 |
| 社媒导入插件 | 导入小红书、公众号、微博等社媒链接 | `00_Inbox/手动剪藏` 或 `01_Sources/社媒内容` | 需要平台账号权限或手动粘贴链接 |
| Feishu CLI | 导入飞书文档、会议纪要、团队资料 | `01_Sources/会议纪要`、`01_Sources/旧资料导入` | 需要飞书 API 或账号授权 |
| Docxer 插件 | Word/飞书导出文档转 Markdown | `01_Sources/旧资料导入` | 需要插件包或替代转换工具 |
| Importer 插件 | Notion、Apple Notes、旧笔记库迁移 | `01_Sources/旧资料导入` | Obsidian 核心 Importer 已启用；具体格式仍需导入源 |
| Markdown 排版/公众号排版插件 | 文章排版、公众号草稿样式整理 | `04_Outputs/文章` | 需要选择具体排版插件或使用 Codex 生成排版稿 |
| HyperFrames 插件 | 生成短视频摘要、介绍视频结构 | `04_Outputs/视频脚本`、`04_Outputs/视频摘要` | 需要插件包或视频转写工具 |
| 配图 Skill + Image 模型 | 判断配图位置、生成图片、插入 Markdown | `99_Attachments/图片`，文章仍在 `04_Outputs/文章` | Codex 具备图片生成能力；需要具体文章/脚本和风格要求 |

## 必要授权清单

- GitHub：用于抓取 GitHub Trending、仓库热点和推送 Vault。
- RSS/新闻源：需要明确订阅源 URL 或关键词。
- OpenAI/Codex：Claudian 已连接本机 Codex 路径；跨设备需补全 `cliPath`。
- 飞书：需要应用凭证或用户授权。
- 社媒平台：小红书、公众号、微博等通常需要手动剪藏、浏览器扩展或账号授权。
- 图像模型：需要确定使用 Codex 内置生成，还是外部 API。

## 结论

当前 Vault 的“房子”已经搭好，Obsidian 与 Codex 的直接连接也已具备。要达到图片中的完整自动化，还需要补齐外部抓取工具、剪藏入口、账号/API 授权和具体信息源。
