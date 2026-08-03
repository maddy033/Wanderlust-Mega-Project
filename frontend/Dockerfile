# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS frontend-builder

WORKDIR /app

# Copy ONLY package.json to ensure clean lockfile generation
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

COPY --from=frontend-builder /app .

# Purge esbuild Go binary from final image
RUN rm -rf node_modules/@esbuild node_modules/vite/node_modules/@esbuild

COPY .env.docker .env.local

EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host"]