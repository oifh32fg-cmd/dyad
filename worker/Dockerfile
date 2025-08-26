# syntax=docker/dockerfile:1
ARG NODE_VERSION=22.13.1
FROM node:${NODE_VERSION}-slim AS base
WORKDIR /app

# Install only production dependencies (no package.json present, so skip npm ci)

# Copy application files (no build step, pure JS service)
COPY --link dyad-component-selector-client.js ./
COPY --link dyad-shim.js ./
COPY --link proxy_server.js ./

# If you need stacktrace-js, install it (proxy_server.js tries to load it from ../node_modules)
# But since there's no package.json here, we skip npm install. If needed, mount node_modules at runtime.

# Create non-root user
RUN addgroup --system appgroup && adduser --system --ingroup appgroup appuser
USER appuser

ENV NODE_ENV=production
ENV NODE_OPTIONS="--max-old-space-size=4096"

# Expose the port (proxy_server.js expects a port via workerData, so this is for documentation)
EXPOSE 3000

# Default command (must be overridden with correct workerData via parent process)
CMD ["node", "proxy_server.js"]
