# Silicon Image

Agent 平台的 Docker 基础镜像仓库，采用分层架构，通过 GitHub Actions 手动触发构建并推送到 ghcr.io。

## 镜像层级

```
agent-base         Ubuntu 24.04 + Python3/pip + Node.js LTS + uv + 中文字体 + 常用Python库
  └─ agent-runtime   + Playwright (Chromium, Chrome, Firefox) + 预缓存常用 MCP Servers
```

## 镜像说明

| 镜像 | 用途 | 预装内容 |
|------|------|----------|
| `agent-base` | 轻量级 agent 运行环境 | Python3, pip, Node.js, uv/uvx, git, jq, 中文字体, docx/xlsx/pandas/httpx/websockets 等 |
| `agent-runtime` | 全功能 agent 运行环境 | + Playwright (Chromium, Google Chrome, Firefox) + 预缓存 fetch/filesystem/playwright/dingtalk/amap/memory MCP servers |

## 使用方式

```bash
# 拉取镜像
docker pull ghcr.io/silicongroup/silicon_image/agent-base:latest
docker pull ghcr.io/silicongroup/silicon_image/agent-runtime:latest

# 在 Dockerfile 中引用
FROM ghcr.io/silicongroup/silicon_image/agent-base:1.0.0
FROM ghcr.io/silicongroup/silicon_image/agent-runtime:1.0.0
```

## 版本管理

通过 GitHub Actions 手动触发构建（workflow_dispatch）：

- **留空 version**：自动递增 patch 版本（如 `0.1.2` → `0.1.3`，首次为 `0.1.0`）
- **填写 version**：使用指定版本号（如 `1.0.0`、`2.0.0`）

构建成功后自动打 Git tag（`v{版本号}`），下次构建基于此继续递增。

镜像标签：`{版本号}`、`latest`、`{commit-sha}`

## 添加新镜像

1. 创建 `<image-name>/Dockerfile`
2. 在 `.github/workflows/build-images.yml` 中添加对应的 build job
3. 提交后手动触发构建即可
