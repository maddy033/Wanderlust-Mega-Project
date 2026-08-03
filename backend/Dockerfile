# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS backend-builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run test

# ------------------- Stage 2: Final Stage ------------------------------
FROM node:21-slim

# Upgrade base OS packages
RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy app files from builder
COPY --from=backend-builder /app .

# Prune devDependencies to keep image lean & secure
RUN npm prune --production

COPY .env.docker .env

EXPOSE 8080

CMD ["npm", "start"]