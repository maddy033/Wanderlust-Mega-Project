# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS frontend-builder

WORKDIR /app

# Copy ONLY package.json to ignore stale lockfiles
COPY package.json ./

# Clean fresh install forcing tar upgrade
RUN npm install tar@^7.5.19 && npm install

COPY . .

# ------------------- Stage 2: Final Stage ------------------------------
FROM node:21-slim

# Upgrade base OS packages
RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY --from=frontend-builder /app .

# Delete nested esbuild binaries (both top-level and inside vite)
RUN rm -rf node_modules/@esbuild node_modules/vite/node_modules/@esbuild

COPY .env.docker .env.local

EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host"]