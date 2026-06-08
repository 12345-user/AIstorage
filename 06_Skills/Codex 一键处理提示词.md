# Codex 一键处理提示词

所有提示词都使用 `06_Skills/知识库输入标准与指令识别规范.md` 中的统一字段。优先识别：`CMD`、`PROJECT`、`ACTION`、`OUTPUT`、`TARGET`、`PRIORITY`。

## 初始化知识库

```text
CMD:整理 | PROJECT:X | ACTION:C | OUTPUT:暂无 | TARGET:06_Skills | PRIORITY:高

请根据 AGENTS.md 和 06_Skills/知识库输入标准与指令识别规范.md，检查 Vault 是否缺少文件夹、模板、规则、插件配置台账或统一输入字段。缺少的话直接补齐，并输出已完成和仍需授权的项目。
```

## 整理 Inbox

```text
CMD:整理 | PROJECT:待判断 | ACTION:P/X/S/A/B/C/D | OUTPUT:暂无 | TARGET:00_Inbox | PRIORITY:高

请处理 00_Inbox 中的新资料。先补齐每条资料的标准识别区，再按相关性、新鲜度、价值度、可输出性、关联性打标，并按 PROJECT 和 ACTION 分流。能移动或新建笔记的直接执行。
```

## 生成每日简报

```text
CMD:简报 | PROJECT:X | ACTION:C | OUTPUT:每日简报 | TARGET:05_Review/每日简报 | PRIORITY:高

请读取今天新增或修改的 Inbox、Sources、Projects、Shared 笔记，生成今日简报。简报必须使用 _templates/每日简报模板.md，并包含标准识别区、分项目重点、可输出选题、可做知识卡观点、下一步动作和待确认事项。
```

## 生成短视频脚本

```text
CMD:脚本 | PROJECT:P1 | ACTION:C | OUTPUT:视频脚本 | TARGET:04_Outputs/视频脚本 | PRIORITY:高

请从 P1 客栈项目、热点素材和对标账号脚本中，生成一个适合发布的短视频脚本。脚本必须使用 _templates/短视频脚本模板.md，并同步关联到 10_Projects/01_客栈自媒体脚本与热点/04_脚本成品。
```

## 生成知识卡片

```text
CMD:知识卡 | PROJECT:待判断 | ACTION:A | OUTPUT:暂无 | TARGET:02_Knowledge | PRIORITY:中

请从指定资料笔记中提炼 1-3 张知识卡片。每张卡片只表达一个核心观点，必须使用 _templates/知识卡片模板.md，并链接原始资料和相关主题。
```

## 生成选题

```text
CMD:选题 | PROJECT:待判断 | ACTION:S | OUTPUT:文章/视频脚本/报告/周报 | TARGET:03_Topics/选题池 | PRIORITY:中

请从指定资料或 Inbox 中提炼可执行选题，使用 _templates/选题模板.md，写清目标读者、可用素材、输出形式和下一步动作。
```

## 输出周报

```text
CMD:周报 | PROJECT:X | ACTION:C | OUTPUT:周报 | TARGET:04_Outputs/周报总结 | PRIORITY:高

请汇总本周新增资料、完成输出、项目进展、遗留问题和下周计划，使用 _templates/周报模板.md 生成周报。
```
