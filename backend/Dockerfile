# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS backend-builder

WORKDIR /app

COPY package*.json ./

# Remove old lockfile and install updated tar and mongoose
RUN rm -f package-lock.json && npm install tar@^7.5.19 mongoose@^8.9.5

COPY . .

# ------------------- Stage 2: Final Stage ------------------------------
FROM node:21-slim

# Upgrade base OS packages to clear libgnutls30
RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY --from=backend-builder /app .

COPY .env.docker .env

EXPOSE 8080

CMD ["npm", "start"]