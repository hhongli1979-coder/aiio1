# Build stage
FROM node:22-alpine AS builder

WORKDIR /buildingai

# Install pnpm
RUN npm install -g pnpm@10.20.0

# Copy package files for dependency installation
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./
COPY packages/api/package.json ./packages/api/
COPY packages/cli/package.json ./packages/cli/
COPY packages/core/package.json ./packages/core/
COPY packages/web/buildingai-ui/package.json ./packages/web/buildingai-ui/
COPY packages/web/@buildingai/ ./packages/web/@buildingai/
COPY packages/@buildingai/ ./packages/@buildingai/

# Install dependencies
RUN pnpm install --frozen-lockfile

# Copy source code
COPY . .

# Build project
RUN pnpm build

# Production stage
FROM node:22-alpine AS production

WORKDIR /buildingai

# Install pnpm and pm2
RUN npm install -g pnpm@10.20.0 pm2

# Copy built files from builder
COPY --from=builder /buildingai/package.json /buildingai/pnpm-lock.yaml /buildingai/pnpm-workspace.yaml ./
COPY --from=builder /buildingai/ecosystem.config.js ./
COPY --from=builder /buildingai/node_modules ./node_modules
COPY --from=builder /buildingai/packages ./packages
COPY --from=builder /buildingai/public ./public
COPY --from=builder /buildingai/storage ./storage
COPY --from=builder /buildingai/templates ./templates
COPY --from=builder /buildingai/extensions ./extensions

# Create logs directory
RUN mkdir -p logs/pm2

# Environment variables
ENV NODE_ENV=production
ENV SERVER_PORT=4090

EXPOSE 4090

# Start application
CMD ["pnpm", "pm2:start", "--no-daemon"]
