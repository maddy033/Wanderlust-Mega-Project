# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS frontend-builder

WORKDIR /app

COPY package*.json ./

# Remove old lockfile and install updated tar and esbuild
RUN rm -f package-lock.json && npm install tar@^7.5.19 esbuild@^0.21.5

COPY . .

# ------------------- Stage 2: Final Stage ------------------------------
FROM node:21-slim

# Upgrade base OS packages to clear libgnutls30
RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY --from=frontend-builder /app .

# Remove esbuild Go binary to avoid Trivy Go stdlib finding
RUN rm -rf node_modules/@esbuild/linux-x64

COPY .env.docker .env.local

EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host"]