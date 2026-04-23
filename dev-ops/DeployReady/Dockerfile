# STAGE 1: Build
# This stage installs production dependencies and prepares the application
FROM node:18-alpine AS build

# Set the working directory
WORKDIR /usr/src/app

# Copy package files for dependency installation
COPY app/package*.json ./

# Install only production dependencies to keep the 'node_modules' small
RUN npm ci --only=production

# STAGE 2: Production
# This stage uses a fresh base image to discard any build-time caches or tools
FROM node:18-alpine

# Set the working directory
WORKDIR /usr/src/app

# Copy only the 'node_modules' from the build stage
COPY --from=build /usr/src/app/node_modules ./node_modules

# Copy the application source code from the 'app' directory
COPY app/ .

# Create a non-root user and group for security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Change ownership of the application files to the non-root user
RUN chown -R appuser:appgroup /usr/src/app

# Switch to the non-root user
USER appuser

# Set the default port (overridable via environment variable)
ENV PORT=3000

# Expose the port
EXPOSE 3000

# Command to start the application
CMD ["npm", "start"]
