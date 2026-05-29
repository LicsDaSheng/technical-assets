# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 仓库概述

`technical-assets` 是存放基础设施和运维配置的单体仓库，当前包含 CI/CD 相关的 Docker 镜像定义。

## 目录结构

```
docker/
└── gitlab-runner/
    └── Dockerfile   # python-uv-ci 自动化测试基础镜像
```

## Docker 镜像

### `docker/gitlab-runner/Dockerfile`

- **基础镜像**：Ubuntu 24.04 (Noble Numbat)
- **用途**：`python` 项目的 GitLab CI 自动化测试 Runner
- **关键组件**：
  - OpenCV 运行时依赖（含 GStreamer 视频后端）
  - Python 3.10、3.11、3.12（由 `uv python install` 管理）

### 构建与测试

```bash
# 本地构建（amd64）
docker build -t python-uv-ci:local docker/gitlab-runner/

# 跨平台构建（Apple Silicon → linux/amd64）
docker buildx build --platform linux/amd64 -t python-uv-ci:local docker/gitlab-runner/

# 验证 uv 和 Python 可用
docker run --rm python-uv-ci:local uv --version
docker run --rm python-uv-ci:local uv python list
```

## 重要约定

- `uv` 二进制通过 `COPY --from=ghcr.io/astral-sh/uv:<version>` 引入，而非 curl 安装脚本。原因：Apple Silicon 跨平台构建时 QEMU 模拟层会导致 uv installer 段错误。
- 更新 `uv` 版本时，同步修改 `COPY --from=ghcr.io/astral-sh/uv:<version>` 中的版本号。
- `DEBIAN_FRONTEND=noninteractive` 已全局设置，`apt-get install` 无需额外的 `-y` 交互处理。
