# ---- Build stage ----
FROM node:22-alpine AS build
WORKDIR /app

# Leverage Docker layer caching - only reinstall deps if package files change
COPY package.json package-lock.json ./
RUN npm ci

COPY . .
RUN npm run build

# ---- Serve stage ----
FROM nginx:1.27-alpine

# Angular SPA routing: fall back to index.html for client-side routes
COPY nginx.conf /etc/nginx/conf.d/default.conf

COPY --from=build /app/dist/frontend/browser /usr/share/nginx/html

EXPOSE 80


CMD ["nginx", "-g", "daemon off;"]
