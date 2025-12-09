# Stage 1: Base
FROM node:20-alpine AS base
WORKDIR /app
COPY package*.json ./

# Stage 2: Development
FROM base AS development
ENV NODE_ENV=development
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "run", "dev"]

# Stage 3: Build
FROM base AS build
RUN npm ci --only=production
COPY . .
RUN npm run build

# Stage 4: Production
FROM node:20-alpine AS production
ENV NODE_ENV=production
WORKDIR /app

# Copy only production dependencies
COPY --from=build /app/node_modules ./node_modules
# Copy built application
COPY --from=build /app/dist ./dist
COPY --from=build /app/package*.json ./

# Create non-root user for security
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

USER nodejs

EXPOSE 3000

CMD ["npm", "start"]
