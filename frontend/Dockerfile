# -------------------
# Step 1 : Base
# -------------------
FROM node:22-slim AS base

WORKDIR /app

RUN apt-get update -y \
  && apt-get install -y --no-install-recommends openssl \
  && rm -rf /var/lib/apt/lists/*

COPY package.json package-lock.json* ./

RUN npm ci


# -------------------
# Step 2 : Builder
# -------------------
FROM base AS builder

WORKDIR /app

COPY . .

# Build Nuxt
RUN npm run build

# -------------------
# Step 3 : Production dependencies
# -------------------
FROM node:22-slim AS production-deps

WORKDIR /app

RUN apt-get update -y \
  && apt-get install -y --no-install-recommends openssl \
  && rm -rf /var/lib/apt/lists/*

COPY package.json package-lock.json* ./

RUN npm ci --omit=dev

# -------------------
# Step 5 : Runner
# -------------------
FROM node:22-slim AS runner

WORKDIR /app

RUN apt-get update -y \
  && apt-get install -y --no-install-recommends openssl \
  && rm -rf /var/lib/apt/lists/*

ENV NODE_ENV=production
ENV NITRO_HOST=0.0.0.0
ENV NITRO_PORT=3000
ENV PORT=3000

# Production dependencies only
COPY --from=production-deps /app/node_modules ./node_modules

# Nuxt production output
COPY --from=builder /app/.output ./.output

# Non-root user
RUN addgroup --system --gid 1001 nodejs \
  && adduser --system --uid 1001 nuxt \
  && chmod +x /app/docker-entrypoint.sh

EXPOSE 3000

CMD ["node", ".output/server/index.mjs"]


# -------------------
# Step 6 : Development
# -------------------
FROM node:22-slim AS dev

WORKDIR /app

RUN apt-get update -y \
  && apt-get install -y --no-install-recommends openssl \
  && rm -rf /var/lib/apt/lists/*

ENV NODE_ENV=development
ENV NITRO_HOST=0.0.0.0
ENV NITRO_PORT=3000
ENV HOST=0.0.0.0

COPY package.json package-lock.json* ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "run", "dev"]