ARG PERL_VERSION

ARG CUSTOM_BLOG_IMAGE
ARG CUSTOM_BLOG_VERSION

FROM perl:${PERL_VERSION} AS builder

COPY ./wp-config.php /tmp/app/wp-config.php

ARG DB_NAME
ARG DB_USER
ARG DB_PASSWORD
ARG DB_HOST
ARG AUTH_KEY
ARG SECURE_AUTH_KEY
ARG LOGGED_IN_KEY
ARG NONCE_KEY
ARG AUTH_SALT
ARG SECURE_AUTH_SALT
ARG LOGGED_IN_SALT
ARG NONCE_SALT

RUN cat /tmp/app/wp-config.php | \
    DB_NAME="${DB_NAME}" \
    DB_USER="${DB_USER}" \
    DB_PASSWORD="${DB_PASSWORD}" \
    DB_HOST="${DB_HOST}" \
    AUTH_KEY="${AUTH_KEY}" \
    SECURE_AUTH_KEY="${SECURE_AUTH_KEY}" \
    LOGGED_IN_KEY="${LOGGED_IN_KEY}" \
    NONCE_KEY="${NONCE_KEY}" \
    AUTH_SALT="${AUTH_SALT}" \
    SECURE_AUTH_SALT="${SECURE_AUTH_SALT}" \
    LOGGED_IN_SALT="${LOGGED_IN_SALT}" \
    NONCE_SALT="${NONCE_SALT}" \
    perl -p \
    -e 's/\Q{{DB_NAME}}\E/$ENV{DB_NAME}/g;' \
    -e 's/\Q{{DB_USER}}\E/$ENV{DB_USER}/g;' \
    -e 's/\Q{{DB_PASSWORD}}\E/$ENV{DB_PASSWORD}/g;' \
    -e 's/\Q{{DB_HOST}}\E/$ENV{DB_HOST}/g;' \
    -e 's/\Q{{AUTH_KEY}}\E/$ENV{AUTH_KEY}/g;' \
    -e 's/\Q{{SECURE_AUTH_KEY}}\E/$ENV{SECURE_AUTH_KEY}/g;' \
    -e 's/\Q{{LOGGED_IN_KEY}}\E/$ENV{LOGGED_IN_KEY}/g;' \
    -e 's/\Q{{NONCE_KEY}}\E/$ENV{NONCE_KEY}/g;' \
    -e 's/\Q{{AUTH_SALT}}\E/$ENV{AUTH_SALT}/g;' \
    -e 's/\Q{{SECURE_AUTH_SALT}}\E/$ENV{SECURE_AUTH_SALT}/g;' \
    -e 's/\Q{{LOGGED_IN_SALT}}\E/$ENV{LOGGED_IN_SALT}/g;' \
    -e 's/\Q{{NONCE_SALT}}\E/$ENV{NONCE_SALT}/g;' \
    > /wp-config.php

FROM ${CUSTOM_BLOG_IMAGE}:${CUSTOM_BLOG_VERSION}

COPY . /tmp/app/
COPY --from=builder /wp-config.php /var/www/html/wp-config.php

RUN mv /tmp/app/php.ini $PHP_INI_DIR/conf.d/custom.ini \
 && mv /tmp/app/apache2.conf /etc/apache2/apache2.conf \
 && mv /tmp/app/000-default.conf /etc/apache2/sites-enabled/000-default.config
