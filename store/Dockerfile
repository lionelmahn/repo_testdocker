FROM php:8.2-fpm

# Cài các gói hệ thống
RUN apt-get update && apt-get install -y \
    git \
    curl \
    zip \
    unzip \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    npm \
    nodejs \
    && docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd zip

# Cài Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Tạo thư mục ứng dụng
WORKDIR /var/www

# Copy source code
COPY . .

# Cấp quyền
RUN chown -R www-data:www-data /var/www \
    && chmod -R 755 /var/www/storage

# Cài các dependency Laravel
RUN composer install --no-interaction --prefer-dist --optimize-autoloader

# Cài npm (frontend)
RUN npm install && npm run build

EXPOSE 9000
CMD ["php-fpm"]
