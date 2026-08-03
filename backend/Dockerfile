# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS backend-builder

WORKDIR /app

COPY package.json ./

# Upgrade global npm (patches internal npm/node_modules/tar)
RUN npm install -g npm@latest

# Install fresh dependencies
RUN npm install tar@^7.5.19 && npm install

COPY . .

# ------------------- Stage 2: Final Stage ------------------------------
FROM node:21-slim

# Upgrade base OS packages & update global npm inside final container
RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*
RUN npm install -g npm@latest

WORKDIR /app

COPY --from=backend-builder /app .

COPY .env.docker .env

EXPOSE 8080

CMD ["npm", "start"]