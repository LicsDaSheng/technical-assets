# technical-assets

存放基础设施与运维配置的单体仓库。

## 内容

### Docker 镜像

| 路径 | 镜像名称 | 用途 |
|------|----------|------|
| `docker/gitlab-runner/` | `python-uv-ci` | GitLab CI 自动化测试 Runner |

#### phys-os-robot

基于 Ubuntu 24.04 的自动化测试基础镜像，为 `python` 项目的 CI 流水线提供运行环境。

**包含组件：**

- 构建工具（`build-essential`、`curl`、`git`）
- OpenCV 运行时依赖（含 GStreamer 视频后端）
- [`uv`](https://github.com/astral-sh/uv) 0.7.8（Python 包管理器）
- Python 3.10 / 3.11 / 3.12

**构建：**

```bash
# 本地构建
docker build -t python-uv-ci:local docker/gitlab-runner/

# 跨平台构建（Apple Silicon → linux/amd64）
docker buildx build --platform linux/amd64 -t python-uv-ci:local docker/gitlab-runner/
```

## 许可证

[Apache License 2.0](LICENSE)
