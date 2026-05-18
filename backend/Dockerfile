# Stage 1: Construcción y dependencias
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
# Instalamos solo dependencias de producción y limpiamos la caché para optimizar capas
RUN npm install --production && npm cache clean --force

# Stage 2: Entorno de ejecución ligero y seguro
FROM node:18-alpine AS runner
WORKDIR /app

# Copiamos solo los artefactos necesarios de la etapa anterior
COPY --from=builder /app/node_modules ./node_modules
COPY package.json ./
COPY server.js ./

# Configurar usuario no root integrado en la imagen de node por seguridad
USER node

EXPOSE 3001

CMD ["npm", "start"]
