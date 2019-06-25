# Apache 2.4.x and PHP-FPM running in Docker

## Prerequisites
* [Docker](https://www.docker.com/get-started)

## Get Started

#### Running Apache 2.4.x and PHP-FPM
```
docker-compose up
```

#### Running with phpMyAdmin and MariaDB
```
docker-compose -f docker-compose.yml -f docker-compose.local.yml up
```

## Setup for Laravel

1. Install [Laravel](https://laravel.com/docs/master/installation) into `src` directory.
1. Update document root to `/var/www/html/public` in Apache virtual host [configurations file](./docker/apache-php-fpm/vhosts.conf).