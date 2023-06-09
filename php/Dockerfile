# Выбераем из какого репо берем данные
FROM php:7.4-fpm

# Устанавливаем дополнительные зависимости
RUN apt-get update && apt-get install -y \
    
    # Здесь список зависимостей
    libpng-dev \
    libjpeg-dev \ 
    # Я указал парочку, для работы с PNG и JPEG файлами

    && rm -rf /var/lib/apt/lists/*

# Перенесем код приложения в контейнер
COPY . /var/www/html

# Установим рабоч. директорию
WORKDIR /var/www/html

# запустим php-fpm
CMD ["php-fpm"]