# syntax=docker/dockerfile:1

ARG NODE_VERSION=22.14.0
ARG PNPM_VERSION=10.4.1

# Base image with Node and Corepack
FROM node:${NODE_VERSION} AS base
RUN --mount=type=cache,target=/root/.npm \
    npm install --global corepack@latest
RUN corepack enable && corepack prepare pnpm@${PNPM_VERSION} --activate
WORKDIR /app

# Builder stage
FROM base AS builder
WORKDIR /app
# Only copy package.json and pnpm-lock.yaml for dependency install
COPY --link package.json pnpm-lock.yaml ./
# Set up pnpm cache
ENV PNPM_HOME=/root/.local/share/pnpm
ENV PATH=$PNPM_HOME:$PATH
ENV PNPM_STORE_DIR=/root/.pnpm-store
RUN --mount=type=cache,target=${PNPM_STORE_DIR} \
    pnpm install --frozen-lockfile
# Copy rest of the app (excluding files via .dockerignore)
COPY --link . .
# Build the app (Vite build)
RUN pnpm run build

# Production image
FROM node:${NODE_VERSION}-slim AS final
RUN npm install --global corepack@latest
RUN corepack enable && corepack prepare pnpm@${PNPM_VERSION} --activate
WORKDIR /app
# Copy only production dependencies and built output
COPY --from=builder /app/package.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/index.html ./index.html
COPY --from=builder /app/public ./public
ENV NODE_ENV=production
# Create non-root user
RUN addgroup --system appgroup && adduser --system --ingroup appgroup appuser
USER appuser
EXPOSE 3000
CMD ["pnpm", "run", "preview"]
