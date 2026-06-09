# p1horizon项目当前进展

## 标准识别区

- 输入ID：20260609-1251-p1horizon项目当前进展
- 输入时间：2026-06-09 12:51
- 来源类型：本地文件夹 / GitHub / 代码仓库 / 素材资产 / 手动整理
- 来源名称：`D:\AIStorage\P1`
- 作者 / 机构：本地项目资料；Codex 汇总
- 原始日期：2026-06-08 至 2026-06-09
- 原始链接：本地路径 `D:\AIStorage\P1`
- 项目归属：P1
- 内容类型：资料 / 输出 / 复盘
- 目标输出：报告 / 视频脚本 / 暂无
- 处理指令：入库 / 整理 / 复盘
- 分类动作：P / B / C
- 优先级：高
- 状态：已入库
- 标签：#P1 #p1horizon #客栈网站 #客栈自媒体 #项目进展 #素材库 #NewHostelWeb

## 一句话摘要

`P1` 文件夹当前包含客栈/民宿真实影像素材与一个已可构建、可测试的客栈展示网站项目 `Hostel_Web`；网站已实现公开展示端、登录、后台内容管理、tRPC 接口、MySQL/Drizzle 数据模型和无数据库演示数据兜底，下一步重点是补齐真实品牌内容、部署配置、数据库、管理员账号与图片存储方案。

## 原始内容

### 1. 总体判断

`D:\AIStorage\P1` 当前可以视为 P1 项目的一组核心资料：

- 一部分是客栈/民宿线下素材资产，位于 `Video&picture`。
- 一部分是客栈网站产品与代码资产，位于 `Hostel_Web`。
- 项目主题与知识库 P1 定义高度相关：客栈卖点、真实照片、客栈空间展示、活动/人物/物品内容管理、后续自媒体脚本和热点输出都可以从这些材料中抽取。

本次入库标注为：`p1horizon项目当前进展`。这里的 `p1horizon` 可理解为 P1 客栈线上展示与内容运营项目的当前阶段记录。

### 2. P1 文件夹结构

当前一级结构：

```text
D:\AIStorage\P1
├─ Hostel_Web
└─ Video&picture
```

`Hostel_Web` 是完整前后端同仓项目，包含：

```text
Hostel_Web
├─ api                  后端 Hono / tRPC / 认证 / 查询
├─ contracts            前后端共享类型、错误、常量
├─ db                   Drizzle 数据库 schema、relations、seed
├─ dist                 已生成构建产物
├─ docs                 项目报告、图片清单、内容资料清单
├─ node_modules         已安装依赖
├─ public               网站静态图片
├─ src                  React 前端
├─ Dockerfile           容器化配置
├─ README.md            项目说明
├─ package.json         依赖与脚本
└─ vite / tsconfig / eslint / drizzle 等配置
```

`Video&picture` 是客栈/民宿素材资产，按场景分组：

```text
Video&picture
├─ 房间内部环境        14 张 png
├─ 门店部分照片        18 张 jpg
├─ 民宿照片            5 张 jpg + 6 个 mp4
└─ 新修剪压缩          36 张 jpg
```

### 3. 素材资产盘点

本次粗略统计 `Video&picture` 下共有：

- 图片：约 73 张，包含 jpg/png。
- 视频：6 个 mp4。
- 场景：门店、房间、民宿空间、阳台、山景、楼顶风光、招牌、吧台、前台、客房、拍照点、展示架等。

素材目录细分：

| 目录 | 数量 | 内容倾向 | 可用于 |
| --- | ---: | --- | --- |
| `房间内部环境` | 14 | 房间截图/内部环境 | 房型展示、网站房间页、住客体验脚本 |
| `门店部分照片` | 18 | 门店现场照片 | 门店介绍、门头识别、环境展示 |
| `民宿照片` | 11 | 民宿图片和视频 | 宣传片、短视频、网站首页素材 |
| `新修剪压缩` | 36 | 已筛选压缩后的可用图 | 网站首图、空间介绍、图文笔记、短视频封面 |

`新修剪压缩` 目录内素材命名较清晰，已经具备内容运营价值，例如：

- 客房：`102.jpg`、`201.jpg`、`201厕.jpg`、`201（1）.jpg`、`201（2）.jpg`、`202（1）.jpg`、`202（2）.jpg`、`302.jpg`、`303.jpg` 等。
- 空间：`厨房吧台全景.jpg`、`厨房吧台近景.jpg`、`前台展示小摩托车.jpg`、`前台南瓜近景.jpg`。
- 品牌/门店：`客栈招牌.jpg`、`客栈招牌全景.jpg`、`招牌特写.jpg`。
- 外部环境：`阳台.jpg`、`阳台2.jpg`、`阳台3.jpg`、`楼顶风光.jpg`、`山.jpg`、`山2.jpg`、`山3.jpg`、`山4.jpg`、`小巷.jpg`。
- 打卡/氛围：`打卡拍照点近景.jpg`、`灯笼特写.jpg`、`展示架近景.jpg`、`酒吧啤酒特写.jpg`、`豪车展台远景.jpg`、`留宇宝藏近景.jpg`。

初步内容价值判断：

- 适合做网站首页 Hero 的素材：阳台、楼顶风光、客栈招牌全景、山景、打卡拍照点。
- 适合做房型/空间展示的素材：102、201、202、302、303、厨房吧台、前台、阳台。
- 适合做小红书/抖音封面的素材：招牌特写、灯笼特写、打卡拍照点、酒吧啤酒特写、楼顶风光。
- 适合做短视频 B-roll 的素材：民宿照片目录下 6 个 mp4，以及山景/阳台/前台/吧台素材。

### 4. Hostel_Web 项目定位

`Hostel_Web` 是一个客栈展示和内容管理网站，当前项目名在 README 中写作 `NewHostelWeb App`。

技术栈：

- 前端：Vite、React 19、TypeScript、React Router、Tailwind CSS、GSAP、lucide-react。
- 后端：Hono、tRPC。
- 数据库：MySQL、Drizzle ORM。
- 鉴权：本地账号登录；项目内仍保留 Kimi OAuth 相关代码。
- 会话：JWT Cookie。
- 质量检查：TypeScript、Vitest、ESLint。
- 构建：Vite + esbuild。
- 容器化：Dockerfile。

本地仓库状态：

- 本地路径：`D:\AIStorage\P1\Hostel_Web`
- 远程仓库：`git@github.com:12345-user/NewHostelWeb.git`
- 当前分支：`main`
- 最新提交：`5a58c47 Refine hostel site and admin access`
- 工作区：当前检查时未发现未提交改动

### 5. 已实现的前端页面

前端路由定义在 `src/App.tsx`，当前页面包括：

| 路由 | 页面 | 当前用途 |
| --- | --- | --- |
| `/` | `Home` | 首页展示 |
| `/activities` | `Activities` | 活动列表 |
| `/activities/:id` | `ActivityDetail` | 活动详情 |
| `/people` | `People` | 人员/伙伴展示 |
| `/items` | `Items` | 物品/空间/纪念物展示 |
| `/admin` | `Admin` | 管理后台 |
| `/login` | `Login` | 登录页 |
| `*` | `NotFound` | 404 |

公开展示端已覆盖：

- 首页大图 Hero 和视觉展示。
- 最新活动/动态入口。
- 活动、人员、物品三个内容模块。
- 客栈介绍和位置/品牌类信息展示。
- 导航栏和页脚。

管理后台已覆盖：

- 活动管理：新增、编辑、删除。
- 人员管理：新增、编辑、删除。
- 物品管理：新增、编辑、删除。
- 图片选择：浏览器端 FileReader 转为 Data URL。
- 权限文案：负责人可以新增、编辑、删除；管理员只能新增、编辑。

### 6. 已实现的后端接口

tRPC 根路由在 `api/router.ts`：

- `ping`
- `auth`
- `activity`
- `people`
- `item`

认证接口：

- `auth.login`
- `auth.me`
- `auth.logout`

活动接口：

- `activity.list`
- `activity.getById`
- `activity.create`
- `activity.update`
- `activity.delete`

人员接口：

- `people.list`
- `people.getById`
- `people.create`
- `people.update`
- `people.delete`

物品接口：

- `item.list`
- `item.getById`
- `item.create`
- `item.update`
- `item.delete`

当前接口策略：

- 列表和详情为公开查询。
- 新增和编辑使用 `editorQuery`，允许 `owner` 或 `admin`。
- 删除使用 `ownerQuery`，只允许 `owner`。
- 如果没有 `DATABASE_URL`，活动、人员、物品接口会使用 `api/demo-data.ts` 中的内存演示数据兜底。

### 7. 数据库结构

数据库 schema 位于 `db/schema.ts`。

当前表结构：

#### users

用途：用户和权限。

字段：

- `id`
- `unionId`
- `name`
- `email`
- `avatar`
- `role`
- `createdAt`
- `updatedAt`
- `lastSignInAt`

注意：当前 schema 中 `role` 枚举为 `user` / `admin`，但中间件和后台页面已经在使用 `owner` / `admin` 权限分层。这里存在一个需要修正的结构不一致问题。

#### activities

用途：客栈活动/动态。

字段：

- `id`
- `title`
- `date`
- `participants`
- `summary`
- `description`
- `images`
- `createdAt`
- `updatedAt`

#### people

用途：客栈成员/伙伴/主理人信息。

字段：

- `id`
- `name`
- `bio`
- `skills`
- `contact`
- `avatar`
- `images`
- `createdAt`
- `updatedAt`

#### items

用途：客栈物品、空间、房型或纪念物展示。

字段：

- `id`
- `name`
- `date`
- `description`
- `image`
- `createdAt`
- `updatedAt`

### 8. 演示数据状态

项目仍带有演示内容，包括：

- 活动：周末篝火故事会、手工陶艺体验日、欧洲小镇骑行之旅、阅读与茶话会。
- 人员：小雨、老王、阿杰。
- 物品：复古旅行背包、手工陶艺杯具、旅者留言笔记本。
- 图片：`public/images` 下有 hero、activity、person、item 分类图片。

这些内容适合用于开发阶段预览，但上线前需要替换为真实客栈信息和真实图片。

### 9. 环境与配置

README 要求：

- Node.js 20+
- npm 10+
- MySQL
- `.env`

当前本地检查结果：

- `node --version`：`v22.14.0`
- `npm --version`：`10.9.2`
- `node_modules`：已存在
- `dist`：已存在

`.env.example` 需要配置：

- `APP_ID`
- `APP_SECRET`
- `DATABASE_URL`
- `VITE_KIMI_AUTH_URL`
- `VITE_APP_ID`
- `KIMI_AUTH_URL`
- `KIMI_OPEN_URL`
- `OWNER_UNION_ID`

其中：

- `DATABASE_URL` 是 MySQL 连接串。
- `APP_SECRET` 用于 JWT 签名。
- `OWNER_UNION_ID` 用于标识负责人/管理员身份。
- Kimi OAuth 相关配置仍保留，但当前 `auth-router.ts` 中已经出现本地账号密码登录逻辑。

### 10. 验证结果

本次已在 `D:\AIStorage\P1\Hostel_Web` 执行验证：

```powershell
npm run check
npm run test
npm run build
```

结果：

- TypeScript 检查：通过。
- Vitest：1 个测试文件通过，3 个测试用例通过。
- 生产构建：通过。

构建提醒：

- Browserslist / caniuse-lite 数据约 6 个月未更新，建议后续运行 `npx update-browserslist-db@latest`。
- 前端主 JS chunk 约 590.52 kB，超过 Vite 默认 500 kB 提醒阈值，后续可考虑路由级动态导入或手动分包。
- 后端 bundle `dist\boot.js` 约 2.3 MB。

### 11. 当前风险和问题

#### 11.1 权限角色定义不一致

代码中已经出现：

- `ownerQuery`
- `editorQuery`
- `requireAnyRole(["owner", "admin"])`
- 后台页面 `canDelete = user?.role === 'owner'`

但数据库 schema 中 `users.role` 目前为：

```ts
mysqlEnum("role", ["user", "admin"])
```

风险：

- 如果真实数据库使用当前 schema，`owner` 角色无法作为合法枚举写入。
- 负责人删除权限可能无法通过真实数据库状态稳定实现。

建议：

- 将数据库枚举改为 `["user", "admin", "owner"]`，或统一改回 `admin` 单角色策略。
- 同步 contracts 类型、middleware、Admin 页面和 seed/初始化逻辑。

#### 11.2 图片上传仍是临时方案

后台上传目前把图片读取为 base64/Data URL 并存入数据字段。

风险：

- 图片体积较大时会拖慢数据库、接口和页面。
- 不适合真实运营中的大量素材管理。

建议：

- 第一阶段：使用本地 `/uploads` 静态目录。
- 第二阶段：使用 S3 / OSS / COS 等对象存储。
- 数据库只保存图片 URL、宽高、alt、排序等元信息。

#### 11.3 真实内容尚未完全替换

当前网站仍保留示例客栈、示例人员、示例活动和示例物品。

上线前必须补齐：

- 客栈真实名称。
- 一句话定位。
- 真实地址。
- 电话、微信、邮箱。
- 预订入口。
- 房型、价格、入住须知。
- 客房和公共空间真实图片。
- 活动/故事真实文本。
- 主理人/团队介绍。
- 版权和备案信息。

#### 11.4 数据库和部署仍待落地

当前构建可通过，但真正上线还需要：

- MySQL 实例。
- `.env` 私密配置。
- 数据库迁移或 `db:push`。
- 登录账号/负责人角色初始化。
- 部署平台或服务器。
- 域名、HTTPS、备份和日志。

#### 11.5 产品栏目还需要决策

当前 `/items` 是物品/纪念物逻辑，但对真实客栈更可能需要改为：

- 房型展示。
- 客栈空间。
- 设施服务。
- 周边路线。

建议第一版优先把 `/items` 改造成“房间与空间”，这样更贴近转化目标。

### 12. 当前可输出内容

基于 P1 当前资料，已经可以产出：

- 客栈网站项目进展报告。
- 网站上线前资料补齐清单。
- 客栈空间照片分类表。
- 小红书/抖音封面图候选清单。
- 客栈官网首页文案初稿。
- 房型展示文案初稿。
- 3-5 条短视频脚本：
  - “住进山景里的客栈是什么感觉”
  - “一个客栈老板如何用照片搭官网”
  - “客栈最适合拍照的 5 个角落”
  - “房间、阳台、山景：住客第一眼看到什么”
  - “从线下民宿到线上展示页的搭建过程”

### 13. 建议下一步

高优先级：

1. 修正 `owner/admin/user` 权限模型不一致问题。
2. 确定客栈真实名称、定位、地址、联系方式和预订入口。
3. 从 `Video&picture\新修剪压缩` 中挑选首页 Hero、房型、公共空间、活动氛围图。
4. 把 `/items` 决策为“房型展示”或“客栈空间”，并调整字段。
5. 准备 `.env`、MySQL 和管理员账号初始化方案。

中优先级：

1. 将图片存储从 Data URL 改为文件 URL。
2. 更新 Browserslist 数据。
3. 对前端进行路由级代码分割，降低首包体积。
4. 把 Kimi OAuth 与本地账号登录策略统一，避免认证入口混乱。
5. 增加后台表单校验、保存状态、错误提示和删除确认细节。

低优先级：

1. 精简未使用 UI 组件。
2. 建立素材命名规范。
3. 为每张核心图片补充 alt、用途、推荐页面和优先级。
4. 后续把项目报告拆成知识卡和输出稿。

## Codex 五项检查

- 相关性：高。该资料直接服务 P1 客栈自媒体、客栈展示网站、素材整理和后续脚本输出。
- 新鲜度：高。文件夹与代码状态均为 2026-06-08 至 2026-06-09 的近期项目资料。
- 价值度：高。包含真实素材、可运行代码、网站功能、上线风险和下一步计划。
- 可输出性：高。可转化为官网文案、短视频脚本、项目报告、上线清单、素材库和复盘。
- 关联性：高。应关联 `10_Projects/01_客栈自媒体脚本与热点`，也可沉淀到 `20_Shared` 的客栈素材库和 `04_Outputs` 的项目报告/脚本。

## 推荐去向

- 保存路径：`00_Inbox/p1horizon项目当前进展.md`
- 关联笔记：
  - `10_Projects/01_客栈自媒体脚本与热点`
  - `00_Inbox/待补配置资料收集表.md`
  - `06_Skills/知识库输入标准与指令识别规范.md`
- 下一步动作：
  - 将本笔记分流进 `10_Projects/01_客栈自媒体脚本与热点` 作为项目进展记录。
  - 单独建立一份“客栈网站上线资料补齐清单”。
  - 单独建立一份“P1 客栈素材资产索引”。
  - 根据素材生成首批短视频脚本和官网文案。
