FROM node:20-alpine AS builder

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .
RUN npm run build


FROM nginx:1.27-alpine

ENV FRONTEND_PORT=80 \
    BACKEND_HOST=backend \
    BACKEND_PORT=8000

COPY nginx.conf.template /etc/nginx/templates/default.conf.template
COPY --from=builder /app/dist/angular-conduit /usr/share/nginx/html

EXPOSE ${FRONTEND_PORT}
