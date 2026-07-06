# --- Stage 1: Compilation (Builder) ---
FROM alpine AS compilation
WORKDIR /app
# En un entorno con React/Angular aqui se haria el "npm run build"
# Como es vanilla JS/HTML, solo copiamos los archivos a este stage intermedio
COPY index.html app.js ./

# --- Stage 2: Testing ---
FROM compilation AS testing
# Aqui ejecutarias pruebas de frontend (jest/cypress, linting, etc.)
# Simulamos un paso de testing validando que los archivos existen
RUN test -f index.html && test -f app.js

# --- Stage 3: Production ---
FROM nginx:alpine AS production
# Copiar configuracion de NGINX
COPY default.conf /etc/nginx/conf.d/default.conf
# Copiar archivos estaticos desde la etapa de compilacion
COPY --from=compilation /app/index.html /usr/share/nginx/html/
COPY --from=compilation /app/app.js /usr/share/nginx/html/

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
