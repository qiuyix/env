FROM php:7.3-fpm


# 配置时区，安装环境依赖
RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone && apt-get update && apt-get install zip unzip libjpeg-dev libwebp-dev openssl libssl-dev libpng-dev libpcre3 libpcre3-dev -y && apt-get clean &&  rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* 

# 安装php扩展
RUN pecl install -o -f redis && docker-php-ext-enable redis && echo "extension=redis.so" >> docker-php-ext-redis.ini && docker-php-ext-install pdo pdo_mysql gd &&  rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* 

# 安装composer
RUN cd /tmp && curl -Ok https://getcomposer.org/download/1.10.15/composer.phar && mv /tmp/composer.phar /usr/local/bin/composer && chmod +x /usr/local/bin/composer && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# 安装nginx
RUN cd /tmp && curl -Ok http://nginx.org/download/nginx-1.18.0.tar.gz && tar -zxvf nginx-1.18.0.tar.gz && cd nginx-1.18.0 && ./configure --prefix=/usr/local/nginx --user=www-data --group=www-data --with-http_v2_module --with-http_ssl_module --with-http_gzip_static_module --with-http_stub_status_module && make && make install && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* 

# 配置nginx和php-fpm的配置文件
COPY ./conf/nginx.conf /usr/local/nginx/conf/nginx.conf
COPY ./conf/run.sh /run.sh
COPY ./CONF/nginx /etc/init.d/nginx
RUN mkdir /usr/local/nginx/conf/conf.d && chmod +x /run.sh

# 设置工作目录
WORKDIR /var/www/html

# 暴露端口
EXPOSE 80 443

# 启动php-fpm和nginx
ENTRYPOINT ["/run.sh"]