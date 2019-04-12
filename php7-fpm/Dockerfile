FROM centos:7.4.1708

# 更换阿里云镜像
RUN yum -y install wget \
	&& wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo \
	&& wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo \
	&& yum clean all

# 安装依赖
RUN yum -y install gcc \
	gcc-c++ \
	libxml2 \
	libxml2-devel \
	openssl-devel \
	bzip2 \
	bzip2-devel \
	libcurl-devel \
	libjpeg \
	libjpeg-devel \
	libpng-devel \
	libicu-devel \
	libmcrypt-devel \
	freetype-devel \
	libtidy \
	libtidy-devel \
	ImageMagick-devel \
	mhash \
	mhash-devel \
	pcre-devel \
	postgresql-devel \
	make \
	cmake \
	autoconf \
     && yum clean all

# 设置编译变量
ARG VERSION="php-7.0.6"
ARG BASEDIR="/usr/local/php7" 
ARG CONF_DIR="/data/conf/php7"
ARG LOG_DIR="/data/logs/php7"
ARG PID_DIR="/data/run"
ARG PAG_DIR="/root/software"
ARG SCRIPTS="/etc/init.d/php7-fpm"

# 放入配置文件
COPY ./php-fpm.conf $CONF_DIR/
COPY ./php.ini $CONF_DIR/
COPY ./php7-fpm /etc/init.d/

# 编译安装
RUN mkdir -p $LOG_DIR $CONF_DIR $PID_DIR $PAG_DIR \
	&& chmod 777 $PID_DIR \
	&& chmod +x $SCRIPTS \
	&& cd /root/software/ \
	&& wget https://github.com/guoqingg/saltstack/raw/master/service/php7/file/php-7.0.6.tar.gz \
	&& tar xf /root/software/${VERSION}.tar.gz \
	&& cd /root/software/${VERSION} \
	&& ./configure --prefix=${BASEDIR} --sysconfdir=${CONF_DIR} --with-config-file-path=${CONF_DIR} --enable-mysqlnd --with-pdo-mysql=mysqlnd --with-mysqli=mysqlnd --with-iconv --with-freetype-dir --with-jpeg-dir --with-png-dir --enable-zip --with-zlib --with-bz2 --enable-calendar --enable-exif --with-libxml-dir --enable-xml --disable-rpath --disable-short-tags --enable-bcmath --enable-shmop --enable-sysvmsg --enable-sysvsem --enable-sysvshm --with-tidy --enable-inline-optimization --with-curl --enable-mbregex --enable-mbstring --with-mcrypt --with-gd --enable-gd-native-ttf --with-openssl --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-soap --enable-intl --with-pdo-pgsql --enable-fpm \
	&& make -j8 \
	&& make install \
# 安装扩展模块
	&& cd /root/software/ \
	&& wget http://pecl.php.net/get/imagick-3.4.3.tgz \
	&& tar xf imagick-3.4.3.tgz \
	&& cd imagick-3.4.3 \
	&& /usr/local/php7/bin/phpize \
	&& ./configure --with-php-config=${BASEDIR}/bin/php-config \
	&& make \
	&& make install \
	&& cd /root/software/ \
	&& wget http://pecl.php.net/get/redis-3.1.2.tgz \
	&& tar xf redis-3.1.2.tgz \
	&& cd redis-3.1.2 \
	&& ${BASEDIR}/bin/phpize \
	&& ./configure --with-php-config=${BASEDIR}/bin/php-config \
	&& make \
	&& make install \
	&& cd /root/software/${VERSION}/ext/pgsql \
	&& ${BASEDIR}/bin/phpize \
	&& ./configure --with-php-config=${BASEDIR}/bin/php-config \
	&& make \
	&& make install \
	&& ln -sv ${BASEDIR} /usr/local/php \
	&& rm -rf /root/software/* 

