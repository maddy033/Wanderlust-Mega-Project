# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS backend-builder

WORKDIR /app

COPY package*.json ./

# Force regeneration of lockfile to pull new mongoose version and tar override
RUN npm install --package-lock-only && npm update mongoose tar && npm install

COPY . .

# ------------------- Stage 2: Final Stage ------------------------------
FROM node:21-slim

# Upgrade base OS packages
RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY --from=backend-builder /app .

COPY .env.docker .env

EXPOSE 8080

CMD ["npm", "start"]