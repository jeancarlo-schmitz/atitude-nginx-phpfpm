FROM php:8.3-fpm-bookworm

# Ajustar repositórios e instalar dependências + cron
RUN printf '%s\n' \
    "deb https://deb.debian.org/debian bookworm main contrib non-free-firmware" \
    "deb https://deb.debian.org/debian-security bookworm-security main contrib non-free-firmware" \
    "deb https://deb.debian.org/debian bookworm-updates main contrib non-free-firmware" \
    > /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        apt-transport-https \
        ca-certificates \
        libpq-dev \
        libzip-dev \
        unzip \
        git \
        gcc \
        make \
        pkg-config \
        curl \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        cron \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd pdo_pgsql pdo zip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Instalar Composer globalmente
COPY --from=composer:2.6 /usr/bin/composer /usr/bin/composer

# Copiar configurações PHP
COPY php.ini /usr/local/etc/php/php.ini
#COPY www.conf /usr/local/etc/php-fpm.d/www.conf

# Diretório de trabalho
WORKDIR /var/www/html/atitude-recuperajur-service

# Ajustar permissões para PHP-FPM
RUN chown -R www-data:www-data /var/www/html/atitude-recuperajur-service

# Adicionar cron job diretamente em /etc/cron.d/
RUN echo "* * * * * root /usr/bin/php /var/www/html/atitude-recuperajur-service/src/Interfaces/Cli/Cron/cron_scheduler.php >> /var/www/html/log/cron_scheduler.log 2>&1" \
    > /etc/cron.d/recuperajur-cron \
    && chmod 0644 /etc/cron.d/recuperajur-cron

# Expõe porta PHP-FPM
EXPOSE 9000

# Comando padrão -> inicia cron + php-fpm juntos
CMD service cron start && php-fpm