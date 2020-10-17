## How to use
- Copy `.env.example` to `.env`
- Modify the above `.env` and `./nginx/.env` as you need (use default is okay)
- Run `docker-compose up -d`
- Akaunting will run on `http://127.0.0.1:8001`

Above steps should reproduce 3 containers :
- docker_akaunting_php_1
- docker_akaunting_nginx_1
- docker_akaunting_db_1

If you wanted to use https, run these command :
- `docker exec -it docker_akaunting_php_1 php /var/www/akaunting/artisan vendor:publish --provider="Fideloper\Proxy\TrustedProxyServiceProvider"`
- `docker copy ./nginx/akaunting/config/trustedproxy.php docker_akaunting_php_1:/var/www/html/config/`
- 
