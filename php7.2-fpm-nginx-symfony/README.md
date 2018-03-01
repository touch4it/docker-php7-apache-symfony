# Basic setup

## docker-compose.yml

```yaml
version: '2'
services:
  php:
    image: touch4it/php-nginx-symfony:php7.2-fpm-nginx-symfony
    expose:
      - 80
```

## docker-compose.override.yml

```yaml
version: '2'
services:
  php:
    volumes:
      - .:/var/www/html
    ports:
      - 8080:80
```

# Image customization

## Dockerfile

```
FROM touch4it/php-nginx-symfony:php7.2-fpm-nginx-symfony

RUN /bin/su -s /bin/bash -c 'composer install -o -a' www-data
RUN /bin/su -s /bin/bash -c 'composer dump-autoload --optimize --no-dev --classmap-authoritative' www-data
RUN service cron start
```

## docker-vars.ini

```
memory_limit = 256M
```

## cron

```
# leave last two lines empty
5 0 * * * www-data cd /var/www/html && /usr/local/bin/php -d memory_limit=-1 bin/console debug:router


```

## entrypoint.sh

```
#!/usr/bin/env bash

php-fpm &
crond -c /etc/cron.d/ -f &
nginx -g "daemon off;"

```

don't forget to make entrypoint.sh executable `chmod +x entrypoint.sh`

## docker-compose.yml

```yaml
version: '2'
services:
  php:
    build: .
    volumes:
      - ./docker-vars.ini:/usr/local/etc/php/conf.d/docker-vars.ini
      - ./entrypoint.sh:/entrypoint.sh
    expose:
      - 80
```

## docker-compose.yml

```yaml
version: '2'
services:
  php:
    volumes:
      - .:/var/www/html
      - ./cron:/etc/cron.d/cron
    ports:
      - 8080:80
```
