# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS frontend-builder

WORKDIR /app

COPY package*.json ./
# Force lockfile update with overrides
RUN npm install

COPY . .
RUN npm run build

# ------------------- Stage 2: Final Stage ------------------------------
FROM node:21-slim

# Upgrade base OS packages
RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy package files & updated lockfile
COPY --from=frontend-builder /app/package*.json ./
COPY --from=frontend-builder /app/dist ./dist

# Install ONLY production dependencies (prunes esbuild/vite build tools)
RUN npm ci --only=production

COPY .env.docker .env.local

EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host"]