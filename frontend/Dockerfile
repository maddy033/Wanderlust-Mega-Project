# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS frontend-builder

WORKDIR /app

# Copy package files first
COPY package*.json ./

# Install all dependencies (including build tools)
RUN npm install

# Copy source files
COPY . .

# Build assets
RUN npm run build

# ------------------- Stage 2: Final Stage ------------------------------
FROM node:21-slim

# Upgrade base OS packages to clear libgnutls30 & base image vulnerabilities
RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy built app and node_modules directly from builder stage
COPY --from=frontend-builder /app .

# Copy environment file
COPY .env.docker .env.local

EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host"]