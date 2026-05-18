# Stage 1: Recopilación de archivos estáticos
FROM alpine:latest AS assets
WORKDIR /src
COPY index.html app.js default.conf ./

# Stage 2: Servidor Web seguro
FROM nginx:alpine

# Creamos el archivo de PID y damos permisos de directorios al usuario 'nginx' antes de cambiarlo
RUN touch /var/run/nginx.pid && \
    chown -R nginx:nginx /var/run/nginx.pid /usr/share/nginx/html /var/cache/nginx /var/log/nginx /etc/nginx

WORKDIR /usr/share/nginx/html
RUN rm -rf ./*

# Copiamos los archivos desde la etapa de assets
COPY --from=assets /src/index.html /src/app.js ./
COPY --from=assets /src/default.conf /etc/nginx/conf.d/default.conf

# Cambiamos al usuario no root provisto por Nginx
USER nginx

# Exponemos el puerto no privilegiado
EXPOSE 8080