## docker-compose.yml

```yaml
version: '2'
services:
  php:
    image: touch4it/php-nginx-symfony:php7.1-fpm-nginx-dev
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
