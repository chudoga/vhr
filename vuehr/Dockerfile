# ===== Build Stage =====
FROM node:14 AS build

WORKDIR /app

# Copy package files for dependency caching
COPY package.json ./

# Set npm registry (taobao mirror cert expired, use official registry)
RUN npm config set registry https://registry.npmjs.org/

# Prefer prebuilt binaries for native modules
ENV SASS_BINARY_SITE=https://github.com/sass/node-sass/releases/download
ENV npm_config_build_from_source=false

# Install dependencies (lockfile has dead taobao URLs, use npm install to regenerate)
RUN npm install

# Copy all source code
COPY . .

# Build the Vue project for production
RUN npm run build

# ===== Runtime Stage =====
FROM nginx:alpine

# Copy built assets
COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
