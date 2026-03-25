FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive \
    LANG=zh_CN.UTF-8 \
    LANGUAGE=zh_CN:zh \
    LC_ALL=zh_CN.UTF-8

# System packages + Chinese locale/fonts + Python3/pip
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates curl git jq wget unzip locales \
    python3 python3-pip python3-venv python3-dev \
    fonts-noto-cjk fonts-wqy-zenhei \
    && sed -i '/zh_CN.UTF-8/s/^# //g' /etc/locale.gen && locale-gen \
    && ln -s /usr/bin/python3 /usr/bin/python \
    && printf '#!/bin/sh\nexec python3 -m pip "$@"\n' > /usr/bin/pip && chmod +x /usr/bin/pip \
    && rm -rf /var/lib/apt/lists/*

# Node.js LTS
RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | bash - \
    && apt-get install -y --no-install-recommends nodejs \
    && rm -rf /var/lib/apt/lists/*

# uv (Python package manager, provides uvx)
RUN curl -LsSf https://astral.sh/uv/install.sh | sh
ENV PATH="/root/.local/bin:${PATH}"

# Common Python libraries
RUN pip3 install --no-cache-dir --break-system-packages \
    python-docx openpyxl pandas requests httpx pyyaml \
    python-pptx Pillow beautifulsoup4 lxml markdown \
    simplejson msgpack pyzmq websockets

WORKDIR /app
