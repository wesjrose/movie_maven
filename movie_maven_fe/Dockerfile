FROM node:24-alpine AS build
WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .
# vite.config.ts refuses to load without BE_URL; the proxy it configures is
# only used by the dev server; nginx handles /api in this image.
ENV BE_URL=http://backend:8001
RUN npm run build

FROM nginx:1.29-alpine
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
