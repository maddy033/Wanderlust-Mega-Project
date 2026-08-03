# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS backend-builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

# ------------------- Stage 2: Final Stage ------------------------------
FROM node:21-slim

# Upgrade base OS packages
RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy full application from builder
COPY --from=backend-builder /app .

# Copy environment file
COPY .env.docker .env

EXPOSE 8080

CMD ["npm", "start"]