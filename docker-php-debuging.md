# Docker PHP Debuging

### 1. Download opcache clear
We utilize [cachetool](https://github.com/gordalina/cachetool) to clear opcache
```
curl -sLO https://github.com/gordalina/cachetool/releases/latest/download/cachetool.phar
chmod +x cachetool.phar
```
### 2. Copy cachetools to your container
```
docker cp cachetool.phar $(docker ps -f name=YOUR_CONTAINER_NAME -q):/app/cachetool.phar
```

### 3. Backup your original php file from your container into host
```
docker cp $(docker ps -f name=YOUR_CONTAINER_NAME -q):/app/YOUR-PHP-FILE.php YOUR-PHP-FILE.orig.php 
```

### 4. Copy you modified php file into your container
```
docker cp YOUR-PHP-FILE.php  $(docker ps -f name=YOUR_CONTAINER_NAME -q):/app/YOUR-PHP-FILE.php
```

### 5. Run the command below every time you update the php file
```
php cachetool.phar opcache:reset --fcgi=/tmp/heroku.fcgi.5000.sock
```
