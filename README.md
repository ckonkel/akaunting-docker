## How to compose
- Copy `.env.example` to `.env`
- Modify the above `.env` and `./nginx/.env` as you need (use default is okay)
- Run `docker-compose up -d`
- Akaunting will live on `http://127.0.0.1:8001`

Above steps should produce these 3 containers :
- docker_akaunting_php_1
- docker_akaunting_nginx_1
- docker_akaunting_db_1

## Setup for HTTPS

If you wanted to use https, run these commands :
- Publish TrustedProxy vendor config

```
docker exec -it docker_akaunting_php_1 \
    php /var/www/akaunting/artisan vendor:publish \
    --provider="Fideloper\Proxy\TrustedProxyServiceProvider"
```

- Copy `trustedproxy.php` from host to the `php` container

```
docker cp ./nginx/akaunting/config/trustedproxy.php docker_akaunting_php_1:/var/www/akaunting/config/
```
