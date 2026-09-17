FROM node:22.14.0-alpine3.21 AS builder

WORKDIR /build

COPY package.json package-lock.json ./

RUN --mount=type=cache,target=/root/.npm \
    npm ci --cache /root/.npm

COPY . .

RUN NODE_OPTIONS=--openssl-legacy-provider \
    npm run build && rm -rf ./node_modules

# deployment image
FROM nginx:1.31.6-alpine3.24 AS runtime

COPY --from=builder /build/dist/frontend /usr/share/nginx/html

CMD ["nginx", "-g", "daemon off;"]
