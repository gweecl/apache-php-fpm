# Multi-stage builds are a new feature requiring Docker 17.05 or higher on the daemon and client. 

# stage
FROM composer:latest as composer
LABEL stage=intermediate

COPY ./src /app
RUN composer install
# stage END

FROM amazonlinux

RUN yum update -y \
    && yum install -y httpd \
    && amazon-linux-extras install epel php7.3 \
    && yum install -y supervisor php-soap \
    && yum clean all

WORKDIR /var/www/html
COPY --from=composer /app /var/www/html
COPY ./docker/apache-php-fpm/vhosts.conf /etc/httpd/conf.d/vhosts.conf
COPY ./docker/apache-php-fpm/stderr-logs.conf /etc/php-fpm.d/stderr-logs.conf
COPY ./docker/apache-php-fpm/supervisord.ini /etc/supervisord.d/supervisord.ini

# forward request and error logs to docker log collector
# https://docs.docker.com/v17.09/engine/admin/logging/view_container_logs/
RUN ln -sf /dev/stdout /var/log/httpd/web_access_log \
    && ln -sf /dev/stderr /var/log/httpd/web_error_log \
    && ln -sf /dev/stderr /var/log/php-fpm/error.log \
    && ln -sf /dev/stderr /var/log/php-fpm/www-error.log

CMD ["/usr/bin/supervisord", "-n"]