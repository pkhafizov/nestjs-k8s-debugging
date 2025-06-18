# ─── base ───────
FROM node:22-alpine AS base

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

# ─── debug ───────
FROM base AS debug

ENV NODE_ENV=development

EXPOSE 3000 9229

RUN npm install -g @nestjs/cli

CMD ["npm", "run", "start:debug"]

# ─── builder ───────
FROM base AS builder

RUN npm run build
# RUN npm test

# ─── production ───
FROM node:22-alpine AS production

RUN addgroup --system appgroup && adduser --system --ingroup appgroup appuser

WORKDIR /app

COPY --from=builder /app/package*.json ./
RUN npm ci --omit=dev

COPY --from=builder /app/dist ./dist

USER appuser

CMD ["node","dist/main.js"]