# SmartComfy

SmartComfy 是一个基于 ComfyUI 的 AI 工作流管理平台，提供可视化的应用创建和管理能力。

## ✨ 功能特性

### 核心功能
- **应用管理**：创建、编辑、删除 AI 应用
- **工作流导入**：支持从 PNG 图片或 JSON 文件导入 ComfyUI 工作流
- **参数配置**：可视化配置工作流参数，支持文本、数字、选择、复选框等类型
- **一键生成**：基于配置的参数一键生成图片
- **进度实时显示**：实时显示生成进度，支持 WebSocket 实时更新
- **批量处理**：支持上传 ZIP/RAR/7Z 压缩包或 PDF，批量应用工作流

### 安全特性
- **只读模式**：支持通过端口或请求头控制只读访问
- **权限控制**：创建/编辑/删除权限基于访问模式动态控制
- **代码混淆**：服务端代码经过混淆处理

## 🚀 快速开始

### 环境要求
- **Node.js 18 或更高版本**（[下载地址](https://nodejs.org/)）
- **ComfyUI**（[官方仓库](https://github.com/comfyanonymous/ComfyUI)）

### 安装步骤

1. **下载 SmartComfy**：
   - 访问 [Releases 页面](https://github.com/shenhuaxiyuan/SmartComfy/releases)
   - 下载最新版本的 `SmartComfy-vX.X.X.zip`
   - 解压到任意目录

2. **启动 ComfyUI**：
   - 确保 ComfyUI 已启动并运行在 `http://localhost:8188`

3. **启动 SmartComfy**：
   - **完整权限模式**：双击 `start.bat`（推荐，支持创建/编辑/删除应用）
   - **只读模式**：双击 `start-readonly.bat`（仅允许查看和使用应用）

4. **访问应用**：
   - 完整权限：打开浏览器访问 `http://localhost:3001`
   - 只读模式：打开浏览器访问 `http://localhost:3002`
   - 首次运行会自动安装依赖，请耐心等待

### 文件结构

解压后的目录结构：

```
SmartComfy-v1.0.6/
├── start.bat              # Windows 启动脚本
├── start.sh               # Mac/Linux 启动脚本
├── obfuscated-server.js   # 混淆后的服务端代码
├── package.json           # 依赖配置
├── data/                  # 应用数据（自动创建）
├── workflow-pic/          # 工作流图片（自动创建）
├── output_batch/          # 批量任务输出（自动创建）
└── Vue/dist/              # 前端构建产物
```

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

### 批量处理

1. 进入「批量任务」页面
2. 上传 ZIP/RAR/7Z 压缩包或 PDF
3. 选择要使用的应用
4. 点击「开始处理」
5. 等待所有图片处理完成，可下载打包结果

## 🔧 配置说明

### ComfyUI 地址

默认情况下 SmartComfy 假设 ComfyUI 运行在 `http://localhost:8188`。

如需修改，设置环境变量：

**Windows**：
```cmd
set COMFYUI_URL=http://your-comfyui-host:port
start.bat
```

**Mac/Linux**：
```bash
export COMFYUI_URL=http://your-comfyui-host:port
./start.sh
```

### 反向代理配置

#### 只读模式

通过反向代理提供只读访问时，需要在代理中添加请求头 `X-Read-Only: true`。

#### Lucky 反代配置示例

```yaml
- name: SmartComfy (ReadOnly)
  type: http
  listen:
    - host: 0.0.0.0
    - port: 2096
  proxy:
    - url: http://192.168.1.103:3001
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
        proxy_pass http://localhost:3001;
        proxy_set_header X-Read-Only true;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

## ❓ 常见问题

### Q: 启动时提示"未检测到 Node.js"
A: 请先安装 Node.js 18 或更高版本：[https://nodejs.org/](https://nodejs.org/)

### Q: 启动后浏览器访问 3001 端口无响应
A:
1. 检查命令行窗口是否有报错
2. 确认 3001 端口未被其他程序占用
3. 确认 ComfyUI 已启动（如果只用应用管理功能可以暂时不启）

### Q: 批量任务上传 RAR 文件失败
A: 需要系统安装 7-Zip：[https://www.7-zip.org/](https://www.7-zip.org/)

### Q: 如何更新到新版本？
A:
1. 备份 `data/` 目录（包含你的应用配置）
2. 下载新版本 zip 并解压到新目录
3. 把 `data/` 目录复制到新目录
4. 运行 `start.bat` / `start.sh`

## 📄 许可证

SmartComfy Proprietary License

Copyright (c) 2024 shenhuaxiyuan

本软件仅授权供个人或组织内部使用，禁止复制、修改、反向工程或再分发。

## 📞 支持

如有问题或建议，请通过以下方式联系：
- 提交 GitHub Issue
- 发送邮件至 shenhuaxiyuan@163.com

## 📝 更新日志

### v1.0.6
- 优化发布包结构：双击启动脚本即可运行
- 服务端代码混淆，保护核心逻辑
- 整合前端静态文件到 server.js，单端口访问

### v1.0.0
- 初始版本发布
- 支持工作流导入（PNG/JSON）
- 支持参数配置和一键生成
- 支持实时进度显示
- 支持批量任务处理
