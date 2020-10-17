## How to Compose it
- Copy `.env.example` to `.env` and modify as you wish (default is okay)
- Run `docker-compose up -d`
- Setting up email by modifying `./akaunting/.env` (default is okay)
- Run `docker cp akaunting/.env docker_akaunting_php_1:/var/www/akaunting/`
- Run `docker exec -it docker_akaunting_php_1 chmod 755 /var/www/akaunting/.env`
- Akaunting will live on `http://127.0.0.1:8001` (by default configuration)

If you got 500 Server Error, you might need :
`docker exec -it docker_akaunting_php_1 chmod -R 755 /var/www/akaunting/storage/logs`

Above steps should produce these 3 containers (`docker ps`) :
- docker_akaunting_php_1
- docker_akaunting_nginx_1
- docker_akaunting_db_1

## Setup for HTTPS or Reverse Proxy

Run the following commands to use pre-configured trusted proxy configuration :

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

### Nginx

Use the following configuration for Nginx:
```
server {
    server_name akaunting.localhost;
    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://127.0.0.1:8001;
    }
}
```
