FROM node:20-slim AS base
WORKDIR /app
ENV NODE_ENV=production

COPY package*.json ./
RUN npm install --omit=dev --no-progress
COPY src ./src

RUN groupadd -r app && useradd -r -g app app
USER app
EXPOSE 5921

CMD ["npm", "start"]
