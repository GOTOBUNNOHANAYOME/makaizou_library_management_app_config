# dockerイメージ
FROM php:8.1-apache

# vim文字化け対策LANG環境変数を設定
ENV LANG C.UTF-8
ENV LANGUAGE en_US

# 設定ファイルをdockerコンテナ内のPHP、Apacheに読み込ませる
COPY php.ini /usr/local/etc/php/
COPY 000-default.conf /etc/apache2/sites-enabled/

# Composerのインストール
RUN cd /usr/bin && curl -s http://getcomposer.org/installer | php && ln -s /usr/bin/composer.phar /usr/bin/composer

# # Nodejs インストール
# RUN curl -sL https://deb.nodesource.com/setup_16.x | bash - \ 
#   && apt-get update \
#   && apt-get install -y nodejs

# ミドルウェアインストール
RUN apt-get update \
&& apt-get install -y \
git \
zip \
unzip \
vim \
libzip-dev \
libpng-dev \
libpq-dev \
zlib1g-dev \
libjpeg62-turbo-dev \
imagemagick \
libmagickwand-dev \
&& docker-php-ext-install pdo_mysql gd zip bcmath \
&& docker-php-ext-configure gd --with-jpeg \
&& docker-php-ext-install -j$(nproc) gd \
&& pecl install imagick \
&& docker-php-ext-enable imagick

# Laravelで必要になるmodRewriteを有効化する
RUN mv /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enabled
RUN /bin/sh -c a2enmod rewrite