# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS backend-builder

WORKDIR /app

COPY package*.json ./

# Force lockfile regeneration to apply 'overrides' for tar
RUN npm install --package-lock-only && npm install

COPY . .

# ------------------- Stage 2: Final Stage ------------------------------
FROM node:21-slim

# Upgrade base OS packages
RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY --from=backend-builder /app .

# Remove esbuild or unused build binaries from runtime container if present
RUN rm -rf node_modules/@esbuild

COPY .env.docker .env

EXPOSE 8080

CMD ["npm", "start"]