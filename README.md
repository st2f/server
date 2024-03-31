build a server with apache, php fpm, mariadb, adminer

after the environment is up, create a project skeleton like laravel or symfony

## build containers

```bash
# create data dir for mariadb
mkdir data

# build and start containers
docker compose build && docker compose up -d
```

web dir
http://localhost:8000/

adminer 
http://localhost:8080/

![interfaces](https://github.com/st2f/server/assets/66139812/ae21ec8e-6413-4256-90b1-6442bff33897)

## build app

```bash
# open terminal in `appserver-php` container as root
docker exec -u root -it appserver-php bash
```

![terminal](https://github.com/st2f/server/assets/66139812/cc0c17ac-61fd-4493-b274-5ae7b78e006e)

structure inside the container
- /var/www/app : php runs as `app` user 
- /var/www/app/public : web dir
  
```bash
# this is where you run `composer create-project ...`
cd /var/www
# example for laravel
composer create-project laravel/laravel:^11.0 project
# or example for symfony
composer create-project symfony/skeleton:"7.0.*" project
# then replace /app/ with /project/ with the right permissions
rm -Rf app/public && shopt -s dotglob && cp -Rf project/* app/ && chown -R app:app app/
```

composer can be used inside the container and coding on the host volume app/

## modify containers

```bash
# stop and rebuilt if you modify Docker* files
docker compose down
docker compose build && docker compose up -d
```



