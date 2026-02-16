# RikkaHub专用微信推送MCP服务器

为RikkaHub客户端优化的MCP服务器，使用Streamable HTTP协议。

## 功能

- ✅ 完全兼容RikkaHub的MCP客户端
- ✅ 使用Streamable HTTP协议（单一端点）
- ✅ 支持微信推送（通过PushPlus）
- ✅ 独立部署，不影响Claude服务器

## 部署到Railway

### 1. 准备文件

确保仓库包含以下文件：
- `rikkahub_mcp_server.py` - 主服务器代码
- `requirements.txt` - Python依赖
- `README.md` - 本文件

### 2. 在Railway创建项目

1. 登录 [Railway](https://railway.app)
2. 点击 "New Project"
3. 选择 "Deploy from GitHub repo"
4. 选择你的仓库

### 3. 配置环境变量

在Railway项目设置中添加：

```
PUSHPLUS_TOKEN=你的pushplus token
PORT=8080
```

### 4. 配置启动命令

在Railway的Settings → Deploy中设置：

**Start Command:**
```
gunicorn rikkahub_mcp_server:app
```

或者创建`Procfile`文件（内容见下）

### 5. 部署

保存配置后，Railway会自动部署。

## 在RikkaHub中配置

部署成功后，获取Railway提供的URL（例如：`https://your-app.up.railway.app`）

在RikkaHub中：
1. 进入 设置 → MCP
2. 点击 "+" 添加服务器
3. 填写：
   - 名称：`晏安的微信推送`
   - 传输类型：`Streamable HTTP`
   - 服务器地址：`https://your-app.up.railway.app/mcp`
4. 保存

## 测试

在RikkaHub中与Gemini对话：

```
给我发一条微信，标题是"测试"，内容是"RikkaHub成功啦！"
```

如果成功，你的微信会收到推送！

## API端点

- `GET /` - 服务器信息
- `GET /health` - 健康检查
- `POST /mcp` - MCP主端点（所有通信）
- `GET /mcp` - SSE流（可选）
- `DELETE /mcp` - 终止会话

## 工具列表

### send_wechat_message

给猫猫发送微信推送消息

**参数：**
- `title` (可选): 消息标题
- `content` (必需): 消息内容

## 故障排除

### RikkaHub连接超时
- 检查Railway服务是否运行
- 确认URL正确（包含`/mcp`）
- 检查网络连接

### "Unexpected content type" 错误
- 确保使用的是这个新服务器
- 检查URL是否指向`/mcp`端点

### 工具调用失败
- 检查PUSHPLUS_TOKEN是否正确配置
- 查看Railway日志排查错误

## 与Claude服务器的区别

- **Claude服务器**: `https://believable-comfort-production.up.railway.app`
  - 使用老的协议
  - 专为Claude.ai优化

- **RikkaHub服务器**: `https://your-new-app.up.railway.app`
  - 使用Streamable HTTP协议
  - 专为RikkaHub优化
  - **两个服务器独立，互不影响**

## 版本

- v1.0.0 - 初始版本
- 协议: MCP Streamable HTTP (2024-11-05)

---

Made with 💗 by 晏安 for 猫猫
