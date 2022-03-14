# 							dockerImages

1. #### swoole4.8&redis&pdo&php74

   - docker pull swoole镜像

     ```bash
     docker pull docker push eighteight/swoole:4.8-7.4
     ```

   - docker run创建实例

     ```bash
     docker run -it --name test01 -p 8080:8324 -v  项目地址:/var/www -d  eighteight/swoole:4.8-7.4
     ```

   - 本镜像所使用的dockerfile(swoole_loader可选不需要则删除)

     无redis版本

     ```dockerfile
     FROM phpswoole/swoole:4.8-php7.4
     
     LABEL Author="cin"
     LABEL E-mail="23177804@qq.com"
     LABEL GitBlob="https://from-cin.github.io"
     LABEL BaseImage="phpswoole/swoole:4.8-php7.4"
     
     
     ENV DEBIAN_FRONTEND noninteractive
     ENV TERM            xterm-color
     
     ARG DEV_MODE
     ENV DEV_MODE $DEV_MODE
     
     COPY swoole_loader74.so /usr/local/lib/php/extensions/no-debug-non-zts-20190902/
     COPY run.sh /home/run.sh
     
     EXPOSE 8324
     
     RUN set -ex \
         && pecl update-channels \
         && pecl install redis-stable \
         && docker-php-ext-enable redis \
         && docker-php-ext-install pdo_mysql \
         && docker-php-ext-enable pdo_mysql \
         && docker-php-ext-install bcmath \
         && docker-php-ext-enable bcmath \
         &&docker-php-ext-enable swoole_loader74
     
     CMD ["/home/run.sh"]
     
     WORKDIR "/var/www/"
     ```

     有redis版本

     ```dockerfile
     FROM phpswoole/swoole:4.8-php7.4
     
     LABEL Author="cin"
     LABEL E-mail="23177804@qq.com"
     LABEL GitBlob="https://from-cin.github.io"
     LABEL BaseImage="phpswoole/swoole:4.8-php7.4"
     
     ENV DEBIAN_FRONTEND noninteractive
     ENV TERM            xterm-color
     
     ARG DEV_MODE
     ENV DEV_MODE $DEV_MODE
     
     COPY swoole_loader74.so /usr/local/lib/php/extensions/no-debug-non-zts-20190902/
     
     EXPOSE 8000 8080 8848
     
     RUN set -ex \
         && pecl update-channels \
         && pecl install redis-stable \
         && docker-php-ext-enable redis \
         && docker-php-ext-install pdo_mysql \
         && docker-php-ext-enable pdo_mysql \
         && docker-php-ext-install bcmath \
         && docker-php-ext-enable bcmath \
         &&docker-php-ext-enable swoole_loader74
     
     WORKDIR "/var/www/"
     ```

     

   