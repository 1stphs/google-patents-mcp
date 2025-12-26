# Google Patents MCP Server

基于 MCP (Model Context Protocol) 的 Google 专利搜索服务器，通过 SerpApi 提供专利搜索功能。

## 功能特性

- 🔍 搜索 Google Patents 专利数据
- 🌐 支持 SSE (Server-Sent Events) 模式
- 🐳 支持 Docker 部署
- ⚡ 支持多种过滤条件（日期、发明人、国家、语言等）

---

## Docker 部署

### 首次部署

```bash
# 克隆项目
git clone <repository-url>
cd google-patents-mcp

# 构建并启动
docker-compose up -d --build
```

### 更新部署

```bash
# 拉取最新代码
git pull

# 重新构建并启动
docker-compose up -d --build
```

### 查看日志

```bash
# 实时查看日志
docker-compose logs -f

# 查看最近 100 行日志
docker-compose logs --tail 100
```

### 停止服务

```bash
docker-compose down
```

### 验证服务状态

```bash
curl http://localhost:8107/health
```

---

## 服务端点

| 端点 | 方法 | 说明 |
|------|------|------|
| `/sse` | GET | SSE 连接端点 |
| `/messages?sessionId=<id>` | POST | 消息接收端点 |
| `/health` | GET | 健康检查端点 |

---

## MCP 客户端配置

### Cherry Studio / 其他 MCP 客户端

```json
{
  "mcpServers": {
    "google-patents-mcp": {
      "url": "http://your-server:8107/sse"
    }
  }
}
```

---

## 环境变量

| 变量名 | 默认值 | 说明 |
|--------|--------|------|
| `SERPAPI_API_KEY` | 已预填 | SerpApi API 密钥 |
| `LOG_LEVEL` | `info` | 日志级别 (debug/info/warn/error) |
| `PORT` | `8107` | 服务端口 |

### 自定义环境变量

创建 `.env` 文件：

```bash
SERPAPI_API_KEY=your_api_key_here
LOG_LEVEL=info
```

---

## 提供的 MCP 工具

### `search_patents`

搜索 Google 专利。

**参数：**

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `q` | string | ✅ | 搜索关键词 |
| `page` | integer | ❌ | 页码，默认 1 |
| `num` | integer | ❌ | 每页结果数，10-100，默认 10 |
| `sort` | string | ❌ | 排序：relevance/new/old |
| `before` | string | ❌ | 最大日期过滤，格式：type:YYYYMMDD |
| `after` | string | ❌ | 最小日期过滤，格式：type:YYYYMMDD |
| `inventor` | string | ❌ | 发明人过滤 |
| `assignee` | string | ❌ | 专利权人过滤 |
| `country` | string | ❌ | 国家代码过滤 (US, CN, JP 等) |
| `language` | string | ❌ | 语言过滤 (ENGLISH, CHINESE 等) |
| `status` | string | ❌ | 状态：GRANT/APPLICATION |
| `type` | string | ❌ | 类型：PATENT/DESIGN |

---

## 本地开发

```bash
# 安装依赖
npm install

# 编译
npm run build

# 启动 SSE 模式
npm run start:sse

# 启动 Stdio 模式
npm start
```

---

## 许可证

MIT License
