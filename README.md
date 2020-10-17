## How to Compose it
- Copy `.env.example` to `.env`
- Modify the above `.env` as you wish (default is okay)
- Modify `./nginx/.env` as you need (default is okay)
- Run `docker-compose up -d`
- Akaunting will live on `http://127.0.0.1:8001` (by default configuration)

Above steps should produce these 3 containers (`docker ps`) :
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

References: [https://akaunting.com/docs/developer-manual/reverse-proxy](https://akaunting.com/docs/developer-manual/reverse-proxy)
