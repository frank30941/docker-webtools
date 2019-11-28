FROM php:7.2-apache-stretch
#SHELL ["/bin/bash","-c"]
LABEL maintainer "frank30941 <frank30941@gmail.com>"
# Set the locale
RUN apt-get clean
ENV TZ=Asia/Taipei
RUN unlink /etc/localtime && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone \
    && dpkg-reconfigure -f noninteractive tzdata \
    && apt-get update \
    && apt-get install -y locales locales-all \
    && touch /usr/share/locale/locale.alias \
    && localedef -i zh_TW -c -f UTF-8 -A /usr/share/locale/locale.alias zh_TW.UTF-8
ENV LANG zh_TW.UTF-8

# Set environment variables
ENV COMPOSER_ALLOW_SUPERUSER=1\
     APP_ENV=development \
    NODE_ENV=development \
    INSTALL_XDEBUG=true \
    INSTALL_SASS=true \
    COMPOSE_TLS_VERSION=TLSv1_2
# Install Ubuntu packages
RUN apt-get install -y \
    build-essential \
    curl \
    git \
    libcurl4-openssl-dev \
    libicu-dev \
    libmcrypt-dev \
#    libmysqlclient-dev \
    default-libmysqlclient-dev\
    libpng-dev \
    mysql-client \
    ruby-full \
    software-properties-common \
    vim \
    zip \
    gnupg

# Install PHP and NodeJS
RUN curl -sL https://deb.nodesource.com/setup_6.x | bash - \
    && apt-get install -y \
    nodejs \
    && pecl install -f mcrypt-1.0.1 \
#    && echo "extension=mcrypt.so" > /usr/local/etc/php/conf.d/mcrypt.ini \
    && docker-php-ext-enable mcrypt \
    && docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd \
#    && php -m \
#    && docker-php-ext-configure mcrypt \
#    && pecl install -f mcrypt-1.0.1 \
#    && echo "extension=mcrypt.so" > /usr/local/etc/php/conf.d/mcrypt.ini \
#    && docker-php-ext-enable mcrypt \
    && docker-php-ext-install  -j$(nproc) \
#    curl \
     #fpm \
    gd \
    intl \
#    json \
#    mbstring \
#    mcrypt \
    mysqli \
    pdo_mysql \
    zip \
    opcache \
    && php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && npm install -g bower \
    && mkdir /run/php \
    && apt-get remove -y --purge software-properties-common \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install Dev dependecies
RUN curl -fsSL 'https://xdebug.org/files/xdebug-2.6.0.tgz' -o xdebug.tar.gz \
    && mkdir -p xdebug \
    && tar -xf xdebug.tar.gz -C xdebug --strip-components=1 \
    && rm xdebug.tar.gz \
    && ( \
    cd xdebug \
    && phpize \
    && ./configure --enable-xdebug \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r xdebug \
    && docker-php-ext-enable xdebug
COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini
RUN su -c "gem install sass"

# Apache configuration
RUN a2enmod rewrite
RUN service apache2 restart && php -m

# Change uid and gid of apache to docker user uid/gid
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data

# Install package dependencies
WORKDIR /var/www/html
#COPY ./composer.json ./package.json ./bower.json ./.bowerrc ./
#RUN mkdir -p /var/www/html/public
#RUN composer install && npm config set prefix "/var/www/html/public/node_modules" && npm install --quiet && bower install --allow-root

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
#    locales \
#    locales-all \
    ttf-wqy-microhei \
    ttf-wqy-zenhei \
    xfonts-wqy \
    apt-utils \
    sudo \
    --no-install-recommends

# Add the vscode debian repo
RUN curl -sSL https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor | apt-key add -
RUN echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list
#ADD https://dl.google.com/linux/direct/google-talkplugin_current_amd64.deb /tmp/google-talkplugin_current_amd64.deb
RUN apt-get update && apt-get -y install \
    code \
    git \
    libasound2 \
    libatk1.0-0 \
    libcairo2 \
    libcups2 \
    libexpat1 \
    libfontconfig1 \
    libfreetype6 \
    libgtk2.0-0 \
    libpango-1.0-0 \
    libx11-xcb1 \
    libxcomposite1 \
    libxcursor1 \
    libxdamage1 \
    libxext6 \
    libxfixes3 \
    libxi6 \
    libxrandr2 \
    libxrender1 \
    libxss1 \
    libxtst6 \
#    libcanberra-gtk* \
#    apt-transport-https \
#    ca-certificates \
#    gnupg \
#    hicolor-icon-theme \
#    libgl1-mesa-dri \
#    libgl1-mesa-glx \
#    libpulse0 \
#    libv4l-0 \
    --no-install-recommends \
#    && curl -sSL https://dl.google.com/linux/linux_signing_key.pub | apt-key add - \
#    && echo "deb [arch=amd64] https://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list \
#    && apt-get update && apt-get install -y \
#    google-chrome-stable \
#    --no-install-recommends \
#    && dpkg -i '/tmp/google-talkplugin_current_amd64.deb' \
    && apt-get purge --auto-remove -y curl \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /src/*.deb

ENV HOME /home/user
RUN useradd -g root -G www-data,audio,video -m -d $HOME user \
    && echo "user ALL=NOPASSWD: ALL">> /etc/sudoers \
    && chown -R user:root $HOME

COPY ./start.sh /usr/local/bin/start.sh
RUN chmod 777 /usr/local/bin/start.sh
WORKDIR $HOME
RUN chmod 777 -R /var/www/html && ln -s /var/www/html $HOME/
ENTRYPOINT []
CMD start.sh
