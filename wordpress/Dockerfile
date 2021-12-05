# wp-cli
FROM wordpress:apache
RUN apt update
RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
 && chmod +x wp-cli.phar \
 && mv wp-cli.phar /usr/local/bin/wp

# composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
