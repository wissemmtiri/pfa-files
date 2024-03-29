# DOCKERFILE FOR THE BACKEND SERVICE OF THE APPLICATION
#============================================
#                   BUILD
#============================================

FROM node:18-alpine AS builder

WORKDIR /app

COPY package.json package-lock.json ./

RUN npm ci

COPY . .

RUN npm run build

#============================================
#                   PROD
#============================================

FROM node:18-alpine

RUN adduser \
    --disabled-password \
    --gecos "" \
    --home /nohome \
    --shell /bin/false \
    --no-create-home \ 
    --uid 1001 \
    app

WORKDIR /app

COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules

EXPOSE 3000

USER app

CMD ["node", "dist/main.js"]