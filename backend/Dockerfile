# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS backend-builder

# Setup the working directory
WORKDIR /app

# Copy package files first for cached layer installation
COPY package*.json ./

# Regenerate lockfile to respect overrides and install dependencies
RUN npm install --package-lock-only && npm i

# Copy remaining application code
COPY . .

# Run tests
RUN npm run test

# ------------------- Stage 2: Final Stage ------------------------------
FROM node:21-slim

# Upgrade base OS packages to patch libgnutls30 & system vulnerabilities
RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*

# Setup the working directory
WORKDIR /app

# Copy the build output from the builder stage
COPY --from=backend-builder /app .

# Copy environment variables
COPY .env.docker .env

# Expose backend port
EXPOSE 8080

# Start the application
CMD ["npm", "start"]