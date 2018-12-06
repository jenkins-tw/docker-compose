FROM centos:7

LABEL maintainer="1055873256@qq.com" description="PHP image"

ENV PHP_VERSION 7.2.12

COPY ./ /root/extensions

RUN rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7 \
&& yum install -y epel-release \
&& rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7 \
&& CONFIG="\
        --prefix=/usr/local/php \
        --with-config-file-path=/etc/php \
        --with-config-file-scan-dir=/etc/php/conf.d \
        --enable-fpm \
        --with-fpm-user=www \
        --with-fpm-group=www \
        --enable-short-tags \
        --enable-mbstring \
        --with-curl \
        --enable-sockets \
        --enable-zip \
        --enable-pcntl \
        --with-bz2 \
        --enable-inline-optimization \
        --with-openssl \
        --with-mhash \
#php7bu 支持--with-mcrypt --with-mysql两个变量
#        --with-mcrypt \
        --enable-mbregex \
        --with-pcre-regex \
#        --with-mysql=mysqlnd \
        --with-mysqli=mysqlnd \
        --with-pdo-mysql=mysqlnd \
        --with-gd \
        --with-jpeg-dir \
        --with-zlib \
        --with-png-dir \
        --disable-rpath \
        --enable-bcmath" \
&& yum install -y \
                gcc \
                gcc-c++ \
                bison \
                re2c \
                libxml2-devel \
                openssl-devel \
                bzip2-devel \
                libcurl-devel \
                libjpeg-devel \
                libpng-devel \
                libmcrypt-devel \
#                unzip \
                libmemcached-devel \
                cyrus-sasl-devel \
                ImageMagick-devel \
                make \
                autoconf \
&& groupadd www \
&& useradd -s /sbin/nologin -M -g www www \
&& tar -zxvf /root/extensions/php-$PHP_VERSION.tar.gz -C /root/extensions/ \
&& cd /root/extensions/php-$PHP_VERSION \
&& ./configure $CONFIG \
&& make -j $(getconf _NPROCESSORS_ONLN) \
&& make install \
&& mkdir -p /etc/php/conf.d \
&& cp /root/extensions/php-$PHP_VERSION/php.ini-development /etc/php/php.ini \
&& cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf \
&& cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf \
&& sed -i 's|^;\?pid\s*=\s*\(.\+\)|pid = \1|g; s|^;\?listen\s*=\s*.\+|listen = 9000|g' /usr/local/php/etc/php-fpm.conf \
&& sed -i 's|^;\?error_log\s*=\s*\(.\+\)|error_log = \1|g; s|^;\?daemonize\s*=\s*.\+|daemonize = no|g' /usr/local/php/etc/php-fpm.conf \
&& sed -i 's|^;\?slowlog\s*=\s*\(.\+\)|slowlog = \1|g; s|^;\?catch_workers_output\s*=\s*.\+|catch_workers_output = yes|g' /usr/local/php/etc/php-fpm.conf \
&& sed -i 's/127.0.0.1/0.0.0.0/g' /usr/local/php/etc/php-fpm.d/www.conf \
&& ln -s /usr/local/php/bin/* /usr/local/bin/ \
&& ln -s /usr/local/php/sbin/* /usr/local/sbin/ \
###igbinary
&& cd /root/extensions/ \
&& tar -zxvf igbinary-2.0.8.tgz \
&& cd igbinary-2.0.8 \
&& /usr/local/php/bin/phpize \
&& ./configure --with-php-config=/usr/local/php/bin/php-config \
&& make -j $(getconf _NPROCESSORS_ONLN) \
&& make install \
###libmemcached
&& cd /root/extensions/ \
&& tar -zxvf libmemcached-1.0.18.tar.gz \
&& cd libmemcached-1.0.18 \
&& ./configure --prefix=/usr/local/libmemcached \
&& make -j $(getconf _NPROCESSORS_ONLN) \
&& make install \
###php-memcached
&& cd /root/extensions/php-memcached \
&& /usr/local/php/bin/phpize \
&& ./configure --with-php-config=/usr/local/php/bin/php-config --with-libmemcached-dir=/usr/local/libmemcached/ \
&& make -j $(getconf _NPROCESSORS_ONLN) \
&& make install \
###cphalcon,php自带缓存
&& cd /root/extensions/ \
&& cd cphalcon/build/php7/64bits/ \
&& /usr/local/php/bin/phpize \
&& ./configure --with-php-config=/usr/local/php/bin/php-config \
&& make -j $(getconf _NPROCESSORS_ONLN) \
&& make install \
###redis
&& cd /root/extensions/ \
&& tar -zxvf redis-4.2.0.tgz \
&& cd redis-4.2.0 \
&& /usr/local/php/bin/phpize \
&& ./configure --with-php-config=/usr/local/php/bin/php-config \
&& make -j $(getconf _NPROCESSORS_ONLN) \
&& make install \
###imagick
&& cd /root/extensions/ \
&& tar -zxvf imagick-3.4.3.tgz \
&& cd imagick-3.4.3 \
&& /usr/local/php/bin/phpize \
&& ./configure --with-php-config=/usr/local/php/bin/php-config \
&& make -j $(getconf _NPROCESSORS_ONLN) \
&& make install \
###
&& cd /root/extensions/php-$PHP_VERSION/ext/bcmath/ \
&& /usr/local/php/bin/phpize \
&& ./configure --with-php-config=/usr/local/php/bin/php-config \
&& make -j $(getconf _NPROCESSORS_ONLN) \
&& make install \
###apcu
&& cd /root/extensions/ \
&& tar -zxvf apcu-5.1.14.tgz \
&& cd apcu-5.1.14 \
&& /usr/local/php/bin/phpize \
&& ./configure --with-php-config=/usr/local/php/bin/php-config \
&& make -j $(getconf _NPROCESSORS_ONLN) \
&& make install \
###mongodb
&& cd /root/extensions/ \
&& tar -zxvf mongodb-1.5.3.tgz \
&& cd mongodb-1.5.3 \
&& /usr/local/php/bin/phpize \
&& ./configure --with-php-config=/usr/local/php/bin/php-config \
&& make -j $(getconf _NPROCESSORS_ONLN) \
&& make install \
&& rm -rf /root/extensions/ \
&& yum clean all

EXPOSE 9000

CMD ["/usr/local/php/sbin/php-fpm", "-F", "-c", "/etc/php/php.ini"]
