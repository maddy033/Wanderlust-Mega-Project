# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS frontend-builder

WORKDIR /app

# Copy ONLY package.json to ignore stale lockfiles
COPY package.json ./

# Force fresh install and upgrade nested tar dependencies
RUN npm install tar@^7.5.19 && npm install

COPY . .

# ------------------- Stage 2: Final Stage ------------------------------
FROM node:21-slim

# Upgrade base OS packages
RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY --from=frontend-builder /app .

# Recursively delete ALL @esbuild binary directories across all node_modules
RUN find node_modules -type d -name "@esbuild" -exec rm -rf {} + 2>/dev/null || true

COPY .env.docker .env.local

EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host"]