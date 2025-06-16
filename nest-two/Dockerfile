# ─── builder ───────
FROM node:22.6.0 AS builder

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm ci

COPY . .
RUN npm run build
# RUN npm test

# ─── debug ───────
FROM node:22.6.0 AS debug
WORKDIR /usr/src/app

# grab *all* your source, built assets, and modules from builder
COPY --from=builder --chown=node:node /usr/src/app ./

EXPOSE 3000 9229
USER node

# this uses your "start:debug" script (which does `nest start --debug --watch`)
CMD ["npm", "run", "start:debug"]

# ─── production ───
FROM node:22-alpine AS production
WORKDIR /usr/src/app

COPY --from=builder /usr/src/app/package*.json ./
RUN npm ci --omit=dev
COPY --from=builder /usr/src/app/dist ./dist

EXPOSE 3000
USER node
CMD ["node", "dist/main.js"]