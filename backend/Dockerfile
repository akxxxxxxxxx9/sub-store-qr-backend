FROM node:24.22.1-bookworm-slim AS build

WORKDIR /app

RUN corepack enable && corepack prepare pnpm@11.0.9 --activate

COPY . .

RUN pnpm install --frozen-lockfile \
    && mkdir -p dist \
    && pnpm bundle:esbuild \
    && pnpm install --prod --frozen-lockfile

FROM node:24.22.1-bookworm-slim

ENV NODE_ENV=production
WORKDIR /app

RUN mkdir -p /data && chown node:node /data

COPY --from=build --chown=node:node /app/dist ./dist
COPY --from=build --chown=node:node /app/node_modules ./node_modules

USER node

EXPOSE 3000
CMD ["node", "dist/sub-store.bundle.js"]
