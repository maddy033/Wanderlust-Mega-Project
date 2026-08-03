# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS frontend-builder

WORKDIR /app

COPY package*.json ./

# Force delete old lockfile and install latest secure versions
RUN rm -f package-lock.json && npm install tar@^7.5.19 esbuild@^0.21.5 && npm install

COPY . .

# ------------------- Stage 2: Final Stage ------------------------------
FROM node:21-slim

# Upgrade base OS packages
RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY --from=frontend-builder /app .

# Remove esbuild binary from final image to fix Go stdlib finding
RUN rm -rf node_modules/@esbuild/linux-x64

COPY .env.docker .env.local

EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host"]