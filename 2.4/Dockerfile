FROM centos:7

MAINTAINER VeDocker version 1.0

USER root

RUN yum update -y && yum install -y \
 unzip \
 openssl \
 openssl-devel \
 curl \
 wget \
 gcc gcc-++ \
 expat-devel \
 && yum clean all
 
RUN groupadd -r apache -g 1000 && useradd -u 1000 -r -g apache -m -d /opt/apache2.4 -c "Apache user" apache
	
RUN cd /opt/apache2.4 \
        && curl -O http://apache.mirrors.ionfish.org//httpd/httpd-2.4.29.tar.gz \
        && curl -O http://mirrors.sonic.net/apache//apr/apr-1.6.3.tar.gz \
        && curl -O http://mirrors.sonic.net/apache//apr/apr-util-1.6.1.tar.gz \
        && tar xf httpd-2.4.29.tar.gz \
        && tar -xf apr-1.6.3.tar.gz -C httpd-2.4.29/srclib/ \
	&& tar -xf apr-util-1.6.1.tar.gz -C httpd-2.4.29/srclib/ \
	&& mv httpd-2.4.29/srclib/apr-1.6.3 httpd-2.4.29/srclib/apr \
	&& mv httpd-2.4.29/srclib/apr-util-1.6.1  httpd-2.4.29/srclib/apr-util \
	&& cd httpd-2.4.29 \
	&& chmod 0755 configure \
	&& ./configure --prefix=/opt/apache2.4 --with-included-apr \
	&& make \
	&& make install \
	&& cd /opt/apache2.4 \
	&& sed -i -e 's/User\ daemon/User\ apache/' -e 's/Group\ daemon/Group\ apache/' conf/httpd.conf \
	&& sed -i 's/Require\ all\ denied/Require\ all\ granted/' conf/httpd.conf \
	&& chown -R apache:apache /opt/apache2.4 \
        && chmod -R 755 /opt/apache2.4 \
        && rm -rf httpd-2.4.29 apr-util-1.6.1.tar.gz apr-1.6.3.tar.gz
        
	
WORKDIR /opt/apache2.4
ADD index.html htdocs/

ENV APACHE_HOME=/opt/apache2.4
	
EXPOSE 80

CMD ["/opt/apache2.4/bin/apachectl", "-D", "FOREGROUND"]
