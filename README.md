# LinkMind

自托管网页资料归档与个人知识库平台。

LinkMind 是一个面向学生、开发者和长期信息收集用户的网页收藏、稍后读与资料归档系统。它可以保存网页链接，自动提取正文，归档网页快照，建立全文搜索，并通过人工智能能力辅助生成摘要、标签和知识卡片。

一个用于长期沉淀技术文章、官方文档、论文资料、题解文章、项目链接和学习笔记的个人知识库。

## 项目简介

在日常学习和开发过程中，我们经常会遇到大量值得保存的网页资料，例如技术博客、官方文档、GitHub 项目、论文、教程、竞赛题解、学校通知和面试资料。

普通收藏夹只能保存链接，但无法解决以下问题：

- 收藏太多之后很难再次找到
- 原网页可能删除、失效或改版
- 网页正文无法统一搜索
- 缺少标签、集合和阅读状态管理
- 不能对内容自动总结和提炼重点
- 无法把零散资料沉淀为长期知识库

LinkMind 的目标是把这些零散链接转化为可搜索、可归档、可阅读、可复盘的个人资料库。

## 核心功能

### 链接收藏

用户可以保存任意网页链接。系统会自动抓取网页标题、描述、封面信息和正文内容，并保存到个人收藏库中。

支持的资料类型包括：

- 技术文章
- 官方文档
- GitHub 仓库
- 论文和 PDF
- 竞赛题解
- 学校通知
- 面试资料
- 工具教程
- 新闻文章

### 网页归档

系统支持保存网页快照，降低链接失效、页面删除或内容改版带来的资料丢失风险。

计划支持的归档内容包括：

- 原始网页内容
- 清洗后的正文
- 页面截图
- PDF 快照
- 历史版本记录

### 稍后读

系统提供适合阅读的正文展示页面，用于集中阅读已保存的网页资料。

计划支持：

- 阅读状态
- 星标收藏
- 正文阅读
- 高亮标注
- 阅读笔记
- 来源链接回溯

### 标签与集合

用户可以通过标签和集合组织资料。

标签适合描述内容属性，例如 Go、Vue、数据库、网络安全、论文、面试等。

集合适合组织主题资料，例如 Go 后端学习、Vue 项目实战、论文阅读、面试准备、日本大学院资料等。

### 全文搜索

系统会为收藏内容建立全文索引，支持快速搜索标题、正文、标签、集合、笔记和摘要。

全文搜索用于解决“收藏了但找不到”的问题。

### 人工智能整理

系统可以调用大模型对网页内容进行辅助整理。

计划支持：

- 自动摘要
- 自动生成标签
- 提取关键知识点
- 生成知识卡片
- 相似资料推荐
- 后续扩展知识库问答

人工智能生成的内容只作为辅助结果，用户可以自行修改和确认。

### 网页变化监控

对于重要页面，系统可以定期检查内容变化。

适合监控：

- 学校通知
- 考试公告
- 竞赛公告
- 官方文档更新
- GitHub 发布页面
- 招聘岗位页面

该功能适合作为后续扩展，不放入第一版核心目标。

## 技术栈

### 前端

- Vue 3
- Vite
- TypeScript
- Pinia
- Vue Router
- Naive UI 或 Element Plus

### 后端

- Go
- Gin
- GORM
- gRPC
- Protobuf
- JWT

### 基础设施

- MySQL 或 PostgreSQL
- Redis
- Meilisearch
- MinIO
- Docker Compose

### 内容处理

- goquery
- readability
- chromedp 或 Playwright
- PDF 解析，后续扩展
- OCR 识别，后续扩展

### 人工智能

- DeepSeek API
- OpenAI 兼容接口
- Ollama 本地模型，后续扩展

## 系统架构

LinkMind 采用前后端分离架构。前端负责用户界面和交互，后端使用 Gin 提供统一接口，内部通过 gRPC 拆分网页抓取、归档、搜索和人工智能处理等能力。

```text
Vue 前端
  |
  | HTTP / SSE
  v
Gin API 网关
  |
  | gRPC
  v
内部服务
  ├── 收藏服务
  ├── 抓取服务
  ├── 归档服务
  ├── 搜索服务
  ├── 人工智能服务
  └── 监控服务
  |
  v
MySQL / Redis / Meilisearch / MinIO
```

## 快速开始

### 环境要求

- Go 1.22 及以上
- Node.js 20 及以上
- Docker
- Docker Compose
- MySQL 或 PostgreSQL
- Redis
- Meilisearch
- MinIO

### 克隆项目

```bash
git clone https://github.com/hllttz/linkmind.git
cd linkmind
```

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

默认访问地址：

```text
前端：http://localhost:5173
后端：http://localhost:8080
```

## 项目结构

```text
linkmind/
├── backend/
│   ├── cmd/
│   ├── api/
│   ├── internal/
│   ├── configs/
│   ├── migrations/
│   └── deployments/
├── web/
│   ├── src/
│   ├── package.json
│   └── vite.config.ts
├── docker-compose.yml
└── README.md
```

其中：

- backend 负责后端服务
- web 负责前端页面
- api 目录存放接口定义
- internal 目录存放后端内部实现
- deployments 目录存放部署相关文件

## 项目灵感

LinkMind 参考了以下自托管书签和网页归档项目的产品方向：

- Karakeep
- Linkwarden
- Wallabag
- Shiori
- Memos

LinkMind 更关注学生和开发者的学习资料整理场景，强调技术资料保存、全文检索、摘要整理和长期知识沉淀。

## 参与贡献

欢迎提交问题、建议和代码改进。

提交代码前请确保：

- 代码已格式化
- 测试能够通过
- 没有混入无关改动
- 新功能有必要说明
- 大型改动先进行讨论

## 许可证

MIT License
