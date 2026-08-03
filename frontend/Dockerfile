# ------------------- Stage 1: Build Stage ------------------------------
FROM node:22 AS frontend-builder

WORKDIR /app

# Copy ONLY package.json to ignore stale lockfiles
COPY package.json ./

# Upgrade npm within supported Node version range
RUN npm install -g npm@10.8.2

# Install dependencies forcing secure tar
RUN npm install tar@^7.5.19 && npm install

COPY . .

# ------------------- Stage 2: Final Stage ------------------------------
FROM node:22-slim

# Upgrade base OS packages
RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY --from=frontend-builder /app .

# Recursively delete all @esbuild binary directories across node_modules to clear Go binary finding
RUN find node_modules -type d -name "@esbuild" -exec rm -rf {} + 2>/dev/null || true

COPY .env.docker .env.local

EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host"]