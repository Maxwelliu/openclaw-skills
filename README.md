---
name: "openclaw-installer"
description: "在本地安装 OpenClaw，包含模型配置和依赖管理。当用户想要安装 OpenClaw 或需要完整的设置指南时调用。"
---
## 开始之前

### 所需信息
在开始安装之前，请准备以下信息：

1. **模型 API 密钥**
   - DeepSeek API 密钥（或您计划使用的其他模型 API 密钥）
   - 其他模型提供商的凭证

2. **消息平台配置**
   - 飞书应用凭证（appId 和 appSecret）（如果您计划使用飞书集成）
   - 其他消息平台的凭证（如果需要）

3. **安装位置**
   - 默认：`E:\TEST\trae_projects\openclaw-cn`
   - 您可以在安装过程中指定自定义位置

## 安装步骤

### 步骤 1：克隆仓库

```bash
# 默认位置
mkdir -p E:\TEST\trae_projects
cd E:\TEST\trae_projects
git clone https://gitee.com/OpenClaw-CN/openclaw-cn.git

# 自定义位置
mkdir -p "YOUR_CUSTOM_PATH"
cd "YOUR_CUSTOM_PATH"
git clone https://gitee.com/OpenClaw-CN/openclaw-cn.git
```

### 步骤 2：安装依赖

```bash
cd openclaw-cn
npm install -g pnpm
pnpm install
```

### 步骤 3：构建项目

```bash
pnpm build
pnpm ui:build
```

### 步骤 4：配置 OpenClaw

创建或编辑位于 `C:\Users\YOUR_USERNAME\.openclaw\openclaw.json` 的配置文件：

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "deepseek/deepseek-chat"
      },
      "workspace": "E:\\TEST\\MyWorkspace"
    }
  },
  "channels": {
    "feishu": {
      "appId": "YOUR_FEISHU_APP_ID",
      "appSecret": "YOUR_FEISHU_APP_SECRET",
      "enabled": true,
      "platform": "feishu",
      "dmPolicy": "open",
      "allowFrom": "*"
    }
  },
  "gateway": {
    "mode": "local",
    "auth": {
      "token": "openclaw-secret-token-2026"
    }
  }
}
```

### 步骤 5：创建启动脚本

创建 `启动OpenClaw.bat`：

```batch
@echo off
cd "E:\TEST\trae_projects\openclaw-cn"
echo 正在启动 OpenClaw 服务...
node dist/index.js gateway --token openclaw-secret-token-2026
pause
```

创建 `检查OpenClaw状态.bat`：

```batch
@echo off
cd "E:\TEST\trae_projects\openclaw-cn"
echo 正在检查 OpenClaw 服务状态...
netstat -ano | findstr :18789
if %errorlevel% equ 0 (
    echo OpenClaw 服务正在运行
    echo 控制界面: http://localhost:18789/?token=openclaw-secret-token-2026
) else (
    echo OpenClaw 服务未运行
)
pause
```

### 步骤 6：启动 OpenClaw

```bash
# 使用批处理文件
双击 启动OpenClaw.bat

# 或直接运行
cd openclaw-cn
node dist/index.js gateway --token openclaw-secret-token-2026
```

## 安装后配置

### 如果您在安装期间未提供 API 密钥

1. 编辑位于 `C:\Users\YOUR_USERNAME\.openclaw\openclaw.json` 的配置文件
2. 添加您的模型 API 密钥和消息平台配置
3. 重启 OpenClaw 使更改生效

### 访问控制界面

打开浏览器并导航至：
`http://localhost:18789/?token=openclaw-secret-token-2026`

### 测试安装

1. 使用启动脚本启动 OpenClaw
2. 使用状态脚本检查状态
3. 通过您配置的消息平台（例如飞书）发送测试消息

## 故障排除

### 常见问题

1. **端口 18789 已被使用**
   - 关闭任何现有的 OpenClaw 实例
   - 使用 `openclaw gateway stop` 停止服务

2. **依赖错误**
   - 确保已安装 pnpm：`npm install -g pnpm`
   - 再次运行 `pnpm install`

3. **配置错误**
   - 检查 `openclaw.json` 文件的语法
   - 确保所有必需字段都已填写

4. **消息平台连接问题**
   - 验证您的应用凭证
   - 检查网络连接

## 更新 OpenClaw

要更新到最新版本：

```bash
cd openclaw-cn
git pull
pnpm install
pnpm build
```

## 卸载

要卸载 OpenClaw：

1. 停止服务：`openclaw gateway stop`
2. 删除安装目录
3. 删除配置目录：`C:\Users\YOUR_USERNAME\.openclaw`

## 支持

如果您在安装过程中遇到任何问题，请参考 OpenClaw 文档或向社区寻求帮助。
