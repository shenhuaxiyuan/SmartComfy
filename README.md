# SmartComfy

SmartComfy 是一个基于 ComfyUI 的 AI 工作流管理平台，提供可视化的应用创建和管理能力。

## ✨ 功能特性

### 核心功能
- **应用管理**：创建、编辑、删除 AI 应用
- **工作流导入**：支持从 PNG 图片或 JSON 文件导入 ComfyUI 工作流
- **参数配置**：可视化配置工作流参数，支持文本、数字、选择、复选框等类型
- **一键生成**：基于配置的参数一键生成图片
- **进度实时显示**：实时显示生成进度，支持 WebSocket 实时更新

### 安全特性
- **只读模式**：支持通过端口或请求头控制只读访问
- **权限控制**：创建/编辑/删除权限基于访问模式动态控制

### 多端口支持
- **5173 端口**：完全权限模式，支持创建、编辑、删除操作
- **5174 端口**：只读模式，仅允许查看和使用应用

## 🚀 快速开始

### 环境要求
- Node.js >= 18
- pnpm >= 8
- ComfyUI >= 1.0

### 安装 Node.js

如果你还没有安装 Node.js，请按照以下步骤操作：

**Windows 用户**：
1. 访问 [Node.js 官网](https://nodejs.org/)
2. 下载 LTS 版本（推荐）
3. 运行安装程序，一路点击"下一步"完成安装
4. 打开命令提示符（CMD）或 PowerShell，输入 `node -v` 和 `npm -v` 确认安装成功

**验证安装**：
```bash
node -v
npm -v
```

### 安装 pnpm

```bash
npm install -g pnpm
```

### 安装 ComfyUI

请先按照 [ComfyUI 官方文档](https://github.com/comfyanonymous/ComfyUI) 安装并启动 ComfyUI。

确保 ComfyUI 默认运行在 `http://localhost:8188`。

### 下载并运行 SmartComfy

1. **下载最新版本**：
   - 访问 [GitHub Releases](https://github.com/shenhuaxiyuan/SmartComfy/releases)
   - 下载最新版本的 `dist.zip` 文件
   - 解压到本地目录

2. **启动后端服务**：
   ```bash
   # 在项目根目录
   npm install
   node server.js
   ```
   后端服务默认运行在 `http://localhost:3001`

3. **启动前端**：
   ```bash
   # 使用 Vite 预览构建产物
   cd Vue
   pnpm install
   pnpm preview
   ```

### 访问地址
- **完全权限**：http://localhost:5173
- **只读模式**：http://localhost:5174

## 📖 使用指南

### 创建应用

1. 点击首页的「创建应用」按钮
2. 选择上传方式：
   - **图片上传**：上传包含 ComfyUI 工作流的 PNG 图片
   - **JSON上传**：上传 ComfyUI 工作流 JSON 文件
3. 配置应用名称和描述
4. 选择需要对外暴露的输入参数
5. 选择输出节点
6. 点击「创建」完成

### 使用应用

1. 在首页点击应用卡片进入应用详情页
2. 填写参数（如提示词、图片等）
3. 点击「生成」按钮
4. 等待生成完成，查看结果

## 🔧 配置说明

### 反向代理配置

#### 只读模式配置

通过反向代理提供只读访问时，需要在代理中添加以下请求头：

| 请求头名称 | 值 | 说明 |
|-----------|-----|------|
| X-Read-Only | true | 标记只读模式 |

#### Lucky 反代配置示例

```yaml
- name: SmartComfy (ReadOnly)
  type: http
  listen:
    - host: 0.0.0.0
    - port: 2096
  proxy:
    - url: http://192.168.1.103:5174
  headers:
    - name: X-Read-Only
      value: true
```

#### Nginx 反代配置示例

```nginx
server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://localhost:5174;
        proxy_set_header X-Read-Only true;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

## 📄 许可证

SmartComfy Proprietary License

Copyright (c) 2024 shenhuaxiyuan

本软件仅授权供个人或组织内部使用，禁止复制、修改、反向工程或再分发。

## 📞 支持

如有问题或建议，请通过以下方式联系：
- 提交 GitHub Issue
- 发送邮件至 shenhuaxiyuan@163.com

## 📝 更新日志

### v1.0.0
- 初始版本发布
- 支持工作流导入（PNG/JSON）
- 支持参数配置和一键生成
- 支持实时进度显示
- 支持多端口模式（5173/5174）
