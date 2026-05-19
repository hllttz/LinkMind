# LinkMind

> 自托管网页资料归档与 AI 知识库平台  
> Self-hosted Bookmark, Web Archive and AI Knowledge Base Platform

LinkMind 是一个面向学生、开发者和信息收集型用户的网页资料归档与稍后读平台。它可以帮助用户保存网页、提取正文、归档快照、管理标签与集合，并通过全文搜索和 AI 摘要把零散链接沉淀为可复用的个人知识库。

本项目参考 Karakeep、Linkwarden 等自托管书签与网页归档项目的产品形态，并结合 Gin、GORM、Vue、gRPC 进行重新设计。

---

## 为什么做这个项目

浏览器收藏夹适合“保存链接”，但不适合长期管理知识资料。

常见问题包括：

- 收藏太多之后很难再次找到
- 原网页可能删除、改版或失效
- 无法对网页正文进行统一搜索
- 无法为文章生成摘要、标签和知识卡片
- 无法把博客、文档、论文、GitHub 仓库等资料系统化整理

LinkMind 的目标是解决这些问题：

> 把互联网上零散的链接，沉淀成自己的可搜索、可归档、可阅读、可总结的知识库。

---

## 功能特性

### 链接收藏

- 保存网页链接
- 自动抓取标题、描述、封面图
- 自动提取正文内容
- 支持星标、归档、阅读状态
- 支持按照标签和集合管理

### 网页归档

- 保存原始网页 HTML
- 保存清洗后的正文内容
- 支持网页快照
- 支持历史版本记录
- 防止链接失效后资料丢失

### 稍后读

- Reader View 阅读模式
- 正文清洗展示
- 阅读状态记录
- 高亮文本
- 阅读笔记
- 来源链接回溯

### 全文搜索

- 搜索标题
- 搜索正文
- 搜索标签
- 搜索集合
- 搜索笔记
- 搜索 AI 摘要

### AI 整理

- 自动摘要
- 自动生成标签
- 自动识别内容类型
- 提取关键知识点
- 生成知识卡片
- 后续支持相似内容推荐与知识库问答

### 网页变化监控

- 定时重新抓取网页
- 对比内容 hash
- 检测标题、正文、关键词变化
- 生成变化记录
- 站内通知提醒

---

## 适用场景

LinkMind 适合保存和整理：

- 技术博客
- GitHub 仓库
- 官方文档
- CTF 题解
- 论文 / PDF
- 学校通知
- 考试公告
- 面试资料
- 视频教程
- 工具文档
- 新闻文章

---

## 技术栈

### 前端

- Vue 3
- Vite
- TypeScript
- Pinia
- Vue Router
- Naive UI / Element Plus
- Axios
- SSE / WebSocket

### 后端

- Go
- Gin
- GORM
- gRPC
- Protobuf
- JWT
- Zap / Logrus

### 基础设施

- MySQL / PostgreSQL
- Redis
- Meilisearch
- MinIO
- Docker Compose

### 内容处理

- goquery
- readability
- chromedp / Playwright
- OCR，可选
- PDF parser，可选

### AI 能力

- DeepSeek API
- OpenAI Compatible API
- Ollama，本地模型可选

---

## 系统架构

```text
Vue Web
  |
  | HTTP / SSE
  v
Gin API Gateway
  |
  | gRPC
  v
Internal Services
  ├── BookmarkService
  ├── CrawlService
  ├── ArchiveService
  ├── SearchService
  ├── AIService
  ├── WatchService
  └── NotifyService
  |
  v
Storage
  ├── MySQL / PostgreSQL
  ├── Redis
  ├── Meilisearch
  └── MinIO
```

---

## 核心模块

### API Gateway

基于 Gin 实现，对前端提供统一 HTTP API。

职责：

- 用户认证
- JWT 鉴权
- RESTful API
- 参数校验
- 统一响应
- 文件上传入口
- SSE 任务进度推送
- 调用内部 gRPC 服务

### BookmarkService

负责收藏核心业务。

职责：

- 创建收藏
- 查询收藏
- 修改收藏
- 删除收藏
- 阅读状态管理
- 标签绑定
- 集合绑定

### CrawlService

负责网页抓取与正文提取。

职责：

- 抓取网页 HTML
- 提取标题、描述、封面图
- 提取正文
- 生成内容 hash
- 处理抓取失败

### ArchiveService

负责网页快照归档。

职责：

- 保存 HTML 快照
- 保存正文快照
- 保存截图 / PDF
- 管理历史版本
- 对接 MinIO

### SearchService

负责全文搜索。

职责：

- 建立索引
- 更新索引
- 删除索引
- 关键词搜索
- 标签和集合筛选

### AIService

负责 AI 内容整理。

职责：

- 生成摘要
- 生成标签
- 提取关键点
- 生成知识卡片

### WatchService

负责网页变化监控。

职责：

- 定时抓取网页
- 对比内容变化
- 生成 diff
- 触发通知

---

## 项目结构

```text
linkmind/
├── backend/
│   ├── cmd/
│   │   ├── api/
│   │   ├── worker/
│   │   ├── crawl-service/
│   │   ├── archive-service/
│   │   ├── search-service/
│   │   └── ai-service/
│   ├── api/
│   │   └── proto/
│   ├── internal/
│   │   ├── config/
│   │   ├── database/
│   │   ├── model/
│   │   ├── handler/
│   │   ├── service/
│   │   ├── grpc/
│   │   ├── crawler/
│   │   ├── archive/
│   │   ├── search/
│   │   ├── ai/
│   │   ├── storage/
│   │   ├── scheduler/
│   │   └── middleware/
│   ├── configs/
│   ├── migrations/
│   └── deployments/
│
├── web/
│   ├── src/
│   │   ├── api/
│   │   ├── components/
│   │   ├── views/
│   │   ├── router/
│   │   ├── stores/
│   │   ├── types/
│   │   └── utils/
│   ├── package.json
│   └── vite.config.ts
│
├── docker-compose.yml
└── README.md
```

---

## API 示例

### 用户认证

```text
POST /api/auth/register
POST /api/auth/login
GET  /api/user/profile
```

### 收藏管理

```text
POST   /api/bookmarks
GET    /api/bookmarks
GET    /api/bookmarks/:id
PUT    /api/bookmarks/:id
DELETE /api/bookmarks/:id
```

### 标签与集合

```text
GET    /api/tags
POST   /api/tags
GET    /api/collections
POST   /api/collections
```

### 归档与快照

```text
POST /api/bookmarks/:id/archive
GET  /api/bookmarks/:id/snapshots
GET  /api/snapshots/:id
```

### AI 能力

```text
POST /api/bookmarks/:id/summarize
POST /api/bookmarks/:id/generate-tags
POST /api/bookmarks/:id/generate-cards
```

### 搜索

```text
GET /api/search?keyword=gin
GET /api/search?tag=Go
GET /api/search?collection=backend
```

### 网页监控

```text
POST /api/bookmarks/:id/watch
GET  /api/bookmarks/:id/change-events
```

---

## 快速开始

### 环境要求

- Go 1.22+
- Node.js 20+
- Docker
- Docker Compose
- MySQL / PostgreSQL
- Redis
- Meilisearch
- MinIO

### 启动基础服务

```bash
docker compose up -d mysql redis meilisearch minio
```

### 启动后端

```bash
cd backend

go mod tidy

go run ./cmd/api
```

### 启动前端

```bash
cd web

npm install

npm run dev
```

### 生成 gRPC 代码

```bash
protoc \
  --go_out=. \
  --go-grpc_out=. \
  api/proto/*.proto
```

---

## Docker Compose 部署

第一版推荐使用 Docker Compose 单机部署。

```text
Nginx
 ├── /      -> Vue 静态资源
 └── /api   -> Gin API Gateway

Gin API Gateway
 ├── gRPC -> CrawlService
 ├── gRPC -> ArchiveService
 ├── gRPC -> SearchService
 └── gRPC -> AIService

MySQL / PostgreSQL
Redis
Meilisearch
MinIO
```

---

## 数据模型概览

核心数据表包括：

- users：用户表
- bookmarks：收藏表
- tags：标签表
- collections：集合表
- snapshots：快照表
- notes：笔记表
- highlights：高亮表
- ai_summaries：AI 摘要表
- knowledge_cards：知识卡片表
- crawl_tasks：抓取任务表
- watch_rules：网页监控规则表
- notifications：通知表

---

## 开发路线图

### Phase 1：基础骨架

- Gin 后端初始化
- Vue 前端初始化
- GORM 数据库连接
- 用户注册登录
- 收藏 CRUD
- 最小 gRPC 调用链

### Phase 2：网页抓取

- URL 抓取
- 标题和描述提取
- 正文提取
- 抓取任务记录
- 前端展示抓取状态

### Phase 3：网页归档

- HTML 快照保存
- 正文快照保存
- MinIO 文件存储
- 历史快照查询

### Phase 4：搜索与标签

- 接入 Meilisearch
- 建立全文索引
- 标签筛选
- 集合筛选
- 搜索结果排序

### Phase 5：AI 摘要

- 接入 DeepSeek / Ollama
- 自动摘要
- 自动标签
- 关键点提取
- 知识卡片生成

### Phase 6：阅读器与笔记

- Reader View
- 阅读状态
- 文本高亮
- 阅读笔记
- 星标收藏

### Phase 7：网页变化监控

- 监控规则
- 定时抓取
- 内容变化对比
- 变化事件记录
- 通知提醒

---

## MVP 范围

第一版建议只完成主链路。

### 必须完成

- 用户登录注册
- 保存链接
- 自动抓取标题和正文
- 标签管理
- 集合管理
- 收藏列表
- 收藏详情
- 网页快照
- 全文搜索
- AI 摘要

### 暂不实现

- 浏览器插件
- 移动端
- 多人协作
- SSO
- 复杂权限
- 向量检索
- 复杂 RAG
- OCR
- PDF 深度解析
- 网页变化监控

---

## 后续扩展

- 浏览器插件
- RSS 自动导入
- PDF 上传与解析
- 图片 OCR
- 网页变化监控
- 向量检索
- AI 问答
- 多人协作集合
- 资料包导出
- 移动端适配
- Webhook 通知
- GitHub 仓库自动解析
- Chrome / Pocket / Linkwarden / Karakeep 数据导入

---

## 项目亮点

- Gin + GORM 实现清晰的后端 API 服务
- gRPC 拆分抓取、归档、搜索、AI 等内部服务
- 支持网页正文提取和快照归档
- 使用 Meilisearch 实现全文搜索
- 使用 MinIO 存储网页归档文件
- 支持 AI 摘要、自动标签和知识卡片生成
- 支持阅读器、高亮笔记和稍后读状态管理
- 后续可扩展网页变化监控、RSS 导入和浏览器插件

---

## 项目命名

当前推荐名称：

```text
LinkMind
```

含义：

> 链接管理 + 知识沉淀 + 智能整理。

其他备选：

- LinkVault
- ArchiveMind
- WebShelf
- ReadNest
- PageKeeper
- KnowledgeVault

---

## License

MIT License
