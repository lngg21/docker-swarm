# ----------------------------------------------------
# Estágio 1: Build e Instalação de Dependências
# ----------------------------------------------------
FROM node:20-alpine AS builder

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm prune --production

# ----------------------------------------------------
# Estágio 2: Imagem Final de Produção
# ----------------------------------------------------
FROM node:20-alpine

WORKDIR /usr/src/app

ENV NODE_ENV=production
ENV PORT=3000

COPY --from=builder /usr/src/app/node_modules ./node_modules
COPY --from=builder /usr/src/app/package*.json ./
COPY --from=builder /usr/src/app ./

USER node

EXPOSE ${PORT}

CMD ["node", "index.js"]