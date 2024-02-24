
```bash
mkdir data
docker-compose build && docker-compose up -d
```

httpd / php fpm
http://localhost:8000/

adminer / mariadb
http://localhost:8080/

default 
MARIADB_DATABASE: test_db
MARIADB_USER: user
MARIADB_PASSWORD: password