FROM nginx:stable-alpine

# Copia o conf para dentro do container
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY www /var/www/certbot

# Ajusta permissões
RUN mkdir -p /var/www/certbot && chown -R nginx:nginx /var/www/certbot