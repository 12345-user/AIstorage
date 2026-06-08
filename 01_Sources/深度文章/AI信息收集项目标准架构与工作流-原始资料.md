# AI 信息收集项目标准架构与工作流

参考对象：Horizon  
整理时间：2026-06-08  
适用场景：AI 博主选题雷达、行业日报、竞品监控、技术趋势监测、社媒舆情观察、开源项目追踪

## 一、简化版概括总结

一个成熟的信息收集项目，本质上是一条“多源采集 -> 标准化 -> 去重 -> 智能筛选 -> 深度补全 -> 内容生成 -> 分发归档”的自动化流水线。

最小可用架构可以概括为 7 层：

1. 信息源层  
   连接 RSS、GitHub、Hacker News、Reddit、Telegram、Twitter/X、搜索引擎、行业数据库等外部来源。

2. 采集层  
   按时间窗口、关键词、账号、频道、仓库、榜单等规则抓取原始内容。

3. 标准化层  
   把不同来源的数据统一成同一种内容结构，例如标题、链接、正文、作者、发布时间、来源类型、互动数据、元信息。

4. 清洗与去重层  
   删除重复链接、合并跨平台重复新闻、过滤低质量或过期内容。

5. AI 分析层  
   使用大模型对内容打分、摘要、分类、标签化、判断重要性，并做主题级去重。

6. 增强与生成层  
   对高价值内容补充背景、相关链接、社区讨论、事件脉络，生成日报、周报、选题清单或推送文本。

7. 输出与分发层  
   保存到本地 Markdown/JSON，发布到 GitHub Pages、飞书、钉钉、Slack、Discord、邮件，或暴露为 MCP/API 给其他 AI 工具调用。

一条标准工作流：

```text
配置加载
  -> 确定时间窗口
  -> 并发抓取多来源
  -> 转换为统一内容模型
  -> URL 去重与内容合并
  -> AI 评分、摘要、打标签
  -> 阈值过滤
  -> 主题级去重
  -> 背景知识补全
  -> 生成日报/选题库
  -> 保存、发布、推送
  -> 记录运行状态与成本
```

对 AI 博主来说，最重要的不是“抓得越多越好”，而是建立稳定的信息源矩阵、让 AI 做第一轮筛选、把高价值内容变成可复用的选题资产。

## 二、标准功能模块

### 1. 配置中心

配置中心负责管理项目如何运行。

应包含：

- AI provider 和模型名称
- API Key 所在环境变量名
- 信息源列表
- 时间窗口
- 抓取数量限制
- AI 评分阈值
- 输出语言
- 推送渠道
- 并发与节流参数
- 可选的代理、base URL、私有 feed token

推荐实践：

- 密钥只放 `.env` 或系统环境变量。
- `config.json` 只保存环境变量名，不保存真实密钥。
- 支持 `${VAR_NAME}` 变量替换，方便配置私有 RSS、Webhook URL、代理地址。
- 将“官方样例配置”和“个人生产配置”分开，例如：
  - `config.example.json`
  - `config.ai-blogger.json`
  - `config.production.json`

### 2. 信息源采集器

采集器是系统的输入口。每一种信息源最好独立成一个 scraper。

常见来源：

| 来源 | 价值 | 密钥需求 | 风险点 |
| --- | --- | --- | --- |
| RSS / Atom | 稳定、低成本、适合博客和公告 | 通常不需要 | 内容质量依赖订阅源 |
| GitHub | 追踪开源项目、Release、开发动态 | 建议 token | 匿名 API 限额低 |
| Hacker News | 技术社区热点 | 不需要 | 噪声较多 |
| Reddit | 社区讨论、用户反馈 | 通常不需要 | JSON 可能受限 |
| Telegram | 中文/海外频道消息 | 不需要，公开频道即可 | 只支持公开频道 |
| Twitter/X | AI 圈实时动态 | 通常依赖第三方服务 | 费用、稳定性、反爬限制 |
| 搜索引擎 | 背景补全和交叉验证 | 视服务而定 | 结果质量不稳定 |
| 行业数据库 | 金融、论文、产品、招聘等垂直数据 | 可能需要 | Provider 差异大 |

采集器应做到：

- 独立开关
- 独立限额
- 独立错误处理
- 失败不拖垮主流程
- 返回统一内容模型

### 3. 统一内容模型

标准内容模型是项目长期可维护的关键。

建议字段：

```text
id              唯一 ID，通常由 source + native_id 组成
source_type     来源类型，例如 rss/github/twitter
title           标题
url             原始链接
content         正文、摘要或评论内容
author          作者、账号或频道
published_at    发布时间
fetched_at      抓取时间
metadata        来源特有信息，例如 star、score、comments、repo、subreddit
ai_score        AI 重要性评分
ai_reason       AI 评分理由
ai_summary      AI 摘要
ai_tags         AI 标签
```

统一模型的好处：

- 后续 AI 分析不关心来源差异。
- 去重、过滤、日报生成可以复用同一套逻辑。
- MCP/API 可以暴露统一数据格式。
- 便于保存为 JSON、数据库或搜索索引。

### 4. 去重与合并

信息收集系统必须解决重复信息问题。

基础去重：

- 规范化 URL
- 去掉 `www.`
- 去掉 URL fragment
- 去掉末尾 `/`
- 合并相同 URL 的内容

进阶去重：

- 同一事件多来源报道合并
- 标题相似度判断
- AI 主题级去重
- 保留评分最高或内容最完整的一条
- 合并评论、互动数据、参考链接

典型策略：

```text
相同 URL -> 直接合并
不同 URL 但同主题 -> AI 判断是否重复
重复项 -> 保留主条目，附加其他来源信息
```

### 5. AI 评分与过滤

AI 层是信息收集系统从“聚合器”升级为“选题雷达”的核心。

评分维度可以包括：

- 是否是重大技术进展
- 是否影响行业方向
- 是否有实用价值
- 是否有新产品、新模型、新论文、新开源项目
- 是否有明显讨论热度
- 是否与用户关注领域相关
- 是否只是营销、重复、低质量内容

推荐评分区间：

```text
9-10 重大突破：模型发布、平台级变化、行业事件
7-8  高价值：重要技术、深度文章、优质项目、关键趋势
5-6  有趣但不紧急：可作为备选素材
3-4  低优先级：普通更新、轻量讨论
0-2  噪声：广告、重复、泛泛而谈
```

推荐阈值：

- 日常宽松选题：`6.0`
- 精品日报：`7.0`
- 高强度筛选：`7.5` 或 `8.0`

### 6. 背景补全与深度加工

只做摘要还不够。高价值内容需要补背景，才能成为可写文章的素材。

背景补全可以包括：

- 这件事是什么
- 为什么重要
- 和过去事件有什么关系
- 涉及哪些公司、模型、论文、项目
- 社区如何评价
- 有哪些争议或风险
- 相关链接和延伸阅读

实现方式：

```text
高分内容
  -> 抽取关键词和实体
  -> 搜索相关资料
  -> 读取搜索结果摘要
  -> 让 AI 生成背景、影响、参考链接
```

注意事项：

- 背景补全应只对高分内容执行，控制成本。
- 搜索结果需要作为参考，不应盲信。
- 重要新闻发布前应人工核验原始链接。

### 7. 内容生成

内容生成层把筛选结果转成可读产物。

常见输出：

- 每日简报
- 每周趋势报告
- AI 博主选题库
- 公众号草稿
- Twitter/X thread 草稿
- 小红书/视频脚本素材
- GitHub Pages 博客文章
- Markdown 知识库

日报结构建议：

```text
标题
总体概览
筛选数量 / 原始数量
目录
每条重点资讯：
  标题
  原链接
  AI 评分
  摘要
  为什么重要
  背景知识
  社区讨论
  参考链接
  标签
```

### 8. 分发与集成

输出不只是保存文件，还要送到用户每天会看的地方。

常见分发方式：

| 渠道 | 适合场景 |
| --- | --- |
| 本地 Markdown | 个人归档、Obsidian、知识库 |
| GitHub Pages | 公开日报站 |
| 邮件 | 订阅式日报 |
| 飞书/钉钉 | 团队群推送 |
| Slack/Discord | 海外社区或团队 |
| Webhook | 接入自动化平台 |
| MCP | 让 AI 助手查询和调度流水线 |
| API | 对外提供服务 |

### 9. 运行记录与可观测性

生产化系统需要知道每次跑了什么。

建议记录：

- run_id
- 开始/结束时间
- 抓取时间窗口
- 各来源抓取数量
- 去重前后数量
- AI 分数分布
- 最终保留数量
- token 用量
- API 错误
- 生成文件路径
- 推送是否成功

MCP 或 API 场景下，建议保存阶段产物：

```text
raw_items.json
scored_items.json
filtered_items.json
enriched_items.json
summary-zh.md
summary-en.md
meta.json
```

## 三、精细版特异性细节收纳

下面是从 Horizon 项目抽象出来的更细实现细节，可作为设计同类项目时的 checklist。

### 1. CLI 与 MCP 双入口

标准信息收集项目可以同时提供两种入口：

- CLI：适合定时任务、GitHub Actions、Docker、手动运行。
- MCP：适合 AI 助手调用，支持分阶段执行、读取中间结果、继续调试。

CLI 更像“一键跑完整日报”：

```text
main.py
  -> load_dotenv
  -> StorageManager.load_config
  -> HorizonOrchestrator.run
```

MCP 更像“把流水线拆成工具”：

```text
hz_fetch_items
hz_score_items
hz_filter_items
hz_enrich_items
hz_generate_summary
hz_run_pipeline
```

### 2. 采集器并发模型

采集阶段适合异步并发：

```text
创建 httpx.AsyncClient
  -> 每个 enabled source 创建一个 task
  -> asyncio.gather 并发执行
  -> 单源失败只记录错误，不中断其他来源
  -> flatten 成统一 item list
```

这样能避免某个来源慢或失败导致整套系统卡死。

### 3. 信息源配置的颗粒度

配置不应只有总开关，最好支持多级开关。

例如：

```text
sources.github[] 每个用户/仓库可单独 enabled
sources.rss[] 每个 feed 可单独 enabled
sources.reddit.enabled 全局开关
sources.reddit.subreddits[] 子版块开关
sources.telegram.channels[] 频道开关
sources.twitter.enabled 总开关
```

这样便于测试和临时屏蔽低质量来源。

### 4. Metadata 保留来源特性

统一模型不能抹掉来源差异。来源特有信息应放在 `metadata`。

例子：

```text
GitHub: repo, event_type, release_tag
HN: score, descendants, discussion_url
Reddit: subreddit, upvote_ratio, comments
Twitter: favorite_count, retweet_count, reply_count, views
RSS: feed_name, category
Telegram: channel, message_id
OpenBB: watchlist, symbols, provider
OSS Insight: stars_gained, primary_language
```

AI 评分时可以把 metadata 里的热度和讨论指标加入 prompt。

### 5. AI 调用抽象

不要把项目写死在单一模型供应商上。

推荐抽象：

```text
AIClient.complete(system, user, temperature, max_tokens)
```

再由不同 provider 实现：

- OpenAI
- Anthropic
- Gemini
- Azure OpenAI
- DeepSeek
- DashScope / Qwen
- Doubao
- MiniMax
- Ollama

好处：

- 成本可控
- 额度不足时可切换
- 国内外环境都能部署
- 方便对比模型筛选质量

### 6. AI 失败回退

AI 调用一定会失败，必须设计回退。

建议：

- 单条分析失败，给 `ai_score=0`
- 保存失败理由
- 不让单条失败中断全局流程
- 解析 JSON 失败时使用默认摘要
- 主题去重失败时跳过去重
- 背景补全失败时保留原始摘要

这类系统更看重“每天稳定产出”，而不是每条都完美。

### 7. 成本控制

AI 信息系统最容易失控的是 token 成本。

控制方式：

- 先抓取，再粗过滤
- 只把正文截断到合理长度
- AI 评分阶段只给必要上下文
- 只有高分内容进入背景补全
- 限制 Twitter 回复抓取数量
- 设置 `analysis_concurrency`
- 设置 `enrichment_concurrency`
- 设置 `throttle_sec`
- 记录 token usage

推荐策略：

```text
原始内容 100 条
  -> AI 粗筛 100 条
  -> 保留 10 条
  -> 只对 10 条做背景补全
  -> 只对 10 条生成详细日报
```

### 8. 主题去重的必要性

URL 去重只能解决“同一个链接”。现实里同一新闻可能被多个媒体、多个账号、多个社区重复发布。

主题去重应使用：

```text
标题 + 标签 + AI 摘要
```

让 AI 判断哪些内容是同一事件，再保留最重要的一条。

### 9. 分阶段产物

复杂流水线最好把中间结果落盘。

推荐阶段：

```text
raw       原始抓取并 URL 去重后的内容
scored    AI 打分后的内容
filtered  阈值过滤和主题去重后的内容
enriched  背景补全后的内容
summary   生成后的日报
```

这样做的好处：

- 方便 debug
- 方便人工复核
- 方便 MCP 调用
- 可以从中间阶段重跑
- 不必每次重新抓取外部来源

### 10. 输出适配不同渠道

Markdown 保存到文件没有问题，但发到聊天工具时会遇到长度、格式、卡片限制。

推荐分发策略：

- 本地文件：保留完整 Markdown
- GitHub Pages：加 front matter
- 飞书/Lark：可用 interactive card 或 collapsible layout
- 钉钉：使用 markdown 消息，注意关键词安全校验
- Slack/Discord：注意消息长度限制
- Webhook：支持模板变量和截断

常见模板变量：

```text
#{date}
#{language}
#{important_items}
#{all_items}
#{result}
#{summary}
#{message_title}
#{item_title}
#{item_url}
#{item_score}
```

### 11. 安全与密钥管理

必须避免把密钥写入 Git。

推荐：

- `.env` 放密钥
- `.gitignore` 忽略 `.env`
- `config.json` 只写 `api_key_env`
- 私有 RSS token 使用 `${VAR_NAME}`
- Webhook URL 使用环境变量
- API Key 泄露后立刻轮换
- GitHub token 使用最小权限

### 12. 自动化部署

常见部署方式：

```text
本地手动运行
Windows Task Scheduler
cron
Docker Compose
GitHub Actions
云服务器定时任务
MCP 客户端按需调用
```

GitHub Actions 场景需要：

- Repository Secrets
- API Key
- GitHub Pages 权限
- 定时 cron
- 生成文件 commit/publish 权限

如果只想“下载和拉取更新，不允许 push”，应禁用 push URL：

```powershell
git remote set-url --push origin DISABLED_PUSH_URL
```

### 13. 面向 AI 博主的特化设计

AI 博主的信息源应覆盖五类：

1. 官方发布  
   OpenAI、Anthropic、Google DeepMind、Meta AI、Microsoft、NVIDIA、Hugging Face。

2. 开源项目  
   LLM 框架、Agent 框架、推理引擎、RAG 工具、MCP 生态、模型仓库。

3. 技术社区  
   Hacker News、Reddit、GitHub Trending、OSS Insight。

4. 关键人物  
   研究员、创业者、产品负责人、开源作者。

5. 二级解读  
   Simon Willison、The Batch、Latent Space、Import AI 等。

日报筛选建议：

```text
模型发布 > 平台能力变化 > 开源项目突破 > 重要论文 > 产品更新 > 社区讨论 > 普通观点
```

## 四、通用标准工作流模板

下面是一套可复用到其他项目的信息收集标准工作流。

```text
1. 初始化
   - 加载 .env
   - 加载 config
   - 校验必要密钥
   - 创建运行目录和 run_id

2. 输入采集
   - 读取启用的信息源
   - 按时间窗口抓取
   - 并发执行 scraper
   - 单源失败不中断

3. 数据标准化
   - 转为统一 ContentItem
   - 保存 raw source metadata
   - 记录 fetched_at

4. 清洗去重
   - URL 规范化
   - 相同链接合并
   - 低质量内容过滤
   - 保存 raw_items.json

5. AI 初筛
   - 构造评分 prompt
   - 调用 AI
   - 解析 JSON
   - 保存 scored_items.json

6. 内容过滤
   - 按 ai_score_threshold 过滤
   - 按分数排序
   - AI 主题去重
   - 保存 filtered_items.json

7. 深度补全
   - 对高分内容搜索背景
   - 汇总参考链接
   - 生成背景、影响、讨论
   - 保存 enriched_items.json

8. 内容生成
   - 生成 zh/en Markdown
   - 生成目录和条目
   - 附带评分、标签、来源
   - 保存 summary.md

9. 分发发布
   - 本地保存
   - 复制到站点目录
   - Webhook 推送
   - 邮件发送
   - MCP/API 暴露

10. 观测与维护
   - 记录 token 用量
   - 记录错误来源
   - 记录各阶段数量
   - 输出运行报告
```

## 五、最小可用版本与完整版对比

### 最小可用版本

```text
RSS + GitHub + Hacker News
  -> 统一内容模型
  -> URL 去重
  -> AI 评分
  -> 阈值过滤
  -> Markdown 日报
```

需要：

- 一个有额度的 AI API Key
- 若使用 GitHub，建议配置 GitHub Token
- 一份真实 RSS/GitHub 信息源配置

### 完整生产版本

```text
RSS + GitHub + HN + Reddit + Telegram + Twitter + 搜索 + 行业数据库
  -> 标准化
  -> URL 去重
  -> AI 评分
  -> 主题去重
  -> 背景补全
  -> 双语日报
  -> GitHub Pages
  -> Webhook/Email
  -> MCP/API
  -> 运行记录和成本统计
```

需要：

- AI API Key 和额度
- GitHub Token
- Apify Token
- Webhook URL
- 邮件授权码
- 定时任务
- 日志和中间产物归档

## 六、评估一个信息收集项目是否完整的 checklist

- 是否支持多来源采集？
- 是否每个来源可以独立开关？
- 是否有统一内容模型？
- 是否能处理重复链接？
- 是否能处理同主题重复事件？
- 是否支持 AI 评分和摘要？
- 是否能配置评分阈值？
- 是否能做背景补全？
- 是否能生成结构化日报？
- 是否支持多语言？
- 是否保存中间阶段结果？
- 是否有失败回退？
- 是否有 token/成本统计？
- 是否支持本地文件输出？
- 是否支持推送渠道？
- 是否支持定时运行？
- 是否保护密钥不入库？
- 是否有测试覆盖？
- 是否有 MCP/API 供其他工具调用？

结论：Horizon 可以作为这类项目的一个比较完整的参考实现。它的价值不只在“能抓取信息”，而在于把采集、筛选、补全、生成、分发和 AI 助手集成做成了可扩展的流水线。
