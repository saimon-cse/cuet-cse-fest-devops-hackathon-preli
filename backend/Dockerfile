FROM node:20-slim AS base
WORKDIR /app


FROM base AS deps
ENV NODE_ENV=development
COPY package*.json ./
RUN npm install --no-progress
COPY tsconfig.json tsconfig.json
COPY src ./src
RUN npm run build
RUN npm prune --production

FROM node:20-slim AS runner
WORKDIR /app
ENV NODE_ENV=production

RUN groupadd -r app && useradd -r -g app app
COPY --from=deps /app/node_modules ./node_modules
COPY --from=deps /app/dist ./dist
COPY package*.json ./
USER app
EXPOSE 3847
HEALTHCHECK --interval=30s --timeout=5s --retries=3 CMD node -e "fetch('http://localhost:3847/api/health').then(res=>process.exit(res.ok?0:1)).catch(()=>process.exit(1))"
CMD ["node", "dist/index.js"]
