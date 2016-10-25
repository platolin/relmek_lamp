FROM ubuntu:14.04
MAINTAINER plato lin <lin.plato@gmail.com>

# Setup environment
ENV DEBIAN_FRONTEND noninteractive

# Update sources
RUN apt-get update -y

# install http
RUN apt-get install -y apache2 vim bash-completion unzip
RUN mkdir -p /var/lock/apache2 /var/run/apache2
RUN a2enmod rewrite
# install mysql
RUN apt-get install -y mysql-client mysql-server
#RUN echo "NETWORKING=yes" > /etc/sysconfig/network
# start mysqld to create initial tables
#RUN service mysqld start

# install php
RUN apt-get install -y php5 php5-mysql php5-dev php5-gd php5-memcache php5-pspell php5-snmp snmp php5-xmlrpc libapache2-mod-php5 php5-cli
#RUN yum install -y php php-mysql php-devel php-gd php-pecl-memcache php-pspell php-snmp php-xmlrpc php-xml

# install supervisord

RUN apt-get install -y supervisor
RUN mkdir -p /var/log/supervisor

# install sshd
RUN apt-get install -y openssh-server openssh-client passwd
RUN mkdir -p /var/run/sshd

#RUN ssh-keygen -q -N "" -t dsa -f /etc/ssh/ssh_host_dsa_key && ssh-keygen -q -N "" -t rsa -f /etc/ssh/ssh_host_rsa_key 
RUN sed -ri 's/PermitRootLogin without-password/PermitRootLogin yes/g' /etc/ssh/sshd_config
RUN echo 'root:changeme' | chpasswd

# Put your own public key at id_rsa.pub for key-based login.
RUN mkdir -p /root/.ssh && touch /root/.ssh/authorized_keys && chmod 700 /root/.ssh
#ADD id_rsa.pub /root/.ssh/authorized_keys

#install smtp 
RUN  apt-get install -y exim4-daemon-light mailutils

# Set the time zone
RUN echo "Asia/Taipei" > /etc/timezone && dpkg-reconfigure -f noninteractive tzdata 


ADD phpinfo.php /var/www/html/
RUN mkdir /etc/apache2/ssl
ADD ./mysql/my.cnf /etc/mysql/my.cnf
ADD ./apache2/000-default.conf /etc/apache2/sites-available
ADD ./apache2/ssl/apache.key /etc/apache2/ssl
ADD ./apache2/ssl/apache.pem /etc/apache2/ssl
ADD supervisord.conf /etc/
EXPOSE 22 80 443

CMD ["supervisord", "-n"]

