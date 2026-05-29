# ============================================================
# phys-os-robot — 自动化测试基础镜像
# 基础镜像：Ubuntu 24.04 (Noble Numbat)
# ============================================================
FROM ubuntu:24.04

# 避免 apt 交互式提示
ENV DEBIAN_FRONTEND=noninteractive

# ── 系统基础依赖 ─────────────────────────────────────────────
RUN apt-get update && apt-get install -y --no-install-recommends \
    # 构建工具
    build-essential \
    curl \
    git \
    ca-certificates \
    # OpenCV 运行时依赖
    libgl1 \
    libglib2.0-0 \
    libsm6 \
    libxrender1 \
    libxext6 \
    libgomp1 \
    # OpenCV GUI / video 后端（测试环境用虚拟显示）
    libgstreamer1.0-0 \
    libgstreamer-plugins-base1.0-0 \
    libavcodec60 \
    libavformat60 \
    libswscale7 \
    && rm -rf /var/lib/apt/lists/*

# ── 安装 uv ──────────────────────────────────────────────────
# 直接从官方镜像复制二进制，避免安装脚本在 QEMU 模拟层触发段错误
# （Apple Silicon 跨平台构建时 uv installer 存在 QEMU 兼容问题）
COPY --from=ghcr.io/astral-sh/uv:0.7.8 /uv /usr/local/bin/uv

# 确认 uv 可用
RUN uv --version
RUN uv python install 3.10 3.11 3.12
