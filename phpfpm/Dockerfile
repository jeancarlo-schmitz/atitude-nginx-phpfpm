FROM php:8.3-fpm-bookworm

# Ajustar repositórios e instalar dependências
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
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd pdo_pgsql pdo zip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Instalar Composer globalmente
COPY --from=composer:2.6 /usr/bin/composer /usr/bin/composer

# Copiar php.ini customizado
COPY php.ini /usr/local/etc/php/php.ini

# Definir diretório de trabalho
WORKDIR /var/www/html/atitude-recuperajur-service
RUN composer install --no-dev --optimize-autoloader

# Ajustar permissões para PHP-FPM
RUN chown -R www-data:www-data /var/www/html/atitude-recuperajur-service

# Expõe porta do PHP-FPM
EXPOSE 9000

# Comando padrão
CMD ["php-fpm"]
