# --- Stage 1: Compilation (Builder) ---
FROM node:18-alpine AS compilation
WORKDIR /app
# Copiar dependencias y descargar todas (incluyendo devDependencies para testing)
COPY package*.json ./
RUN npm install
# Copiar código fuente
COPY . .

# --- Stage 2: Testing ---
FROM compilation AS testing
# Ejecutar los tests definidos en package.json
RUN npm run test

# --- Stage 3: Production ---
FROM node:18-alpine AS production
WORKDIR /app
# Solo instalar dependencias de producción para optimizar peso
COPY package*.json ./
RUN npm install --production
# Copiar el código fuente mínimo necesario desde la etapa de compilación
COPY --from=compilation /app/server.js .

EXPOSE 3001
CMD ["node", "server.js"]