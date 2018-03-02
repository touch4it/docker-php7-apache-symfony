FROM touch4it/docker-php7:php7.1-fpm-nginx

RUN apk add --no-cache grep

COPY nginx.vh.default.conf /etc/nginx/conf.d/default.conf

WORKDIR /var/www/html
