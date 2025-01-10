# Stage 1: Build
FROM node:21 AS backend-builder

# Set up the working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package.json package-lock.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Run tests
RUN npm run test

# Stage 2: Production
FROM node:21-slim

# Set up the working directory
WORKDIR /app

# Copy the built application from the previous stage
COPY --from=backend-builder /app .

# Copy environment variables file
COPY .env.docker .env

# Expose the application port
EXPOSE 8080

# Start the application
CMD ["npm", "start"]
