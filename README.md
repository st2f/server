build a server with apache, php fpm, mariadb, adminer

## start

```bash
# create data dir for mariadb
mkdir data

# build and start containers
docker-compose build && docker-compose up -d
```

web dir
http://localhost:8000/

adminer 
http://localhost:8080/

default 
- MARIADB_DATABASE: test_db
- MARIADB_USER: user
- MARIADB_PASSWORD: password

## modify

```bash
# stop and force a rebuilt if you modify Docker* files
docker-compose down
docker-compose build --no-cache && docker-compose up -d --force-recreate
```



