# Laravel setup using docker

Temporary image with composer to avoid installing composer globally
```
$ docker run --rm -v $(pwd):/app composer install
```

Next
```
$ docker-compose up -d
```

Setup local .env as usual
```
$ cp .env.example .env
```
```
#.env
...
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
# database defined in docker-compose.yml
DB_DATABASE=laravel
# mysql will be persisted in host
# choose username and secure password
# will create manually for first time setup
DB_USERNAME=laraveluser
DB_PASSWORD=mysql_secure_password
...
```

Generate key for sessions and cache config
```
$ docker-compose exec app php artisan key:generate
Application key set successfully.

$ docker-compose exec app php artisan config:cache
Configuration cache cleared!
Configuration cached successfully!
```
Check application running at http://{APP_URL} 

## Creating a User for MySQL
Connect to bash on mysql container
```
$ docker-compose exec db bash
```
Login mysql as root user; find password in docker-composer.yml
```
root@33dd5766bdae:/# mysql -u root -p
```
Add new laraveluser
```
mysql> GRANT ALL ON laravel.* TO 'laraveluser'@'%' IDENTIFIED BY 'your_laravel_db_password';
mysql> FLUSH PRIVILEGES;
mysql> Exit;
```


