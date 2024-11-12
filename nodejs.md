# Stage 1: Build dependencies
FROM node:18-alpine AS builder

# Set working directory and install dependencies
WORKDIR /app
COPY package*.json ./
RUN npm install

# Copy application source code
COPY . .

# Build the application
RUN npm run build

# Stage 2: Runtime environment with only built application
FROM node:18-alpine

# Set working directory and copy built files
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules

# Expose port
EXPOSE 3000

# Run the application
CMD ["node", "dist/app.js"]
