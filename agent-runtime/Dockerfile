ARG BASE_TAG=latest
FROM ghcr.io/silicongroup/silicon_image/agent-base:${BASE_TAG}

# Playwright browsers (Chromium, Google Chrome, Firefox) + system deps
RUN npx -y playwright@1.52.0 install --with-deps chromium chrome firefox

# Playwright Python bindings
RUN pip3 install --no-cache-dir --break-system-packages playwright==1.52.0

# Pre-cache common MCP servers
RUN npx -y @anthropic-ai/mcp-server-fetch@latest --help || true \
    && npx -y @modelcontextprotocol/server-filesystem@latest --help || true \
    && npx -y @playwright/mcp@latest --help || true \
    && npx -y dingtalk-mcp@latest --help || true \
    && npx -y @anthropic-ai/mcp-server-memory@latest --help || true \
    && npx -y @amap/amap-maps-mcp-server@latest --help || true \
    && npm cache clean --force

WORKDIR /app
