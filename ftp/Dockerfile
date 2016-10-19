# debian-based container for vsftpd
# VERSION               0.3
FROM debian:jessie
MAINTAINER plato lin  "lin.plato@gmail.com"

RUN apt-get update && apt-get install -y --no-install-recommends vsftpd
RUN apt-get clean

RUN echo "local_enable=YES" >> /etc/vsftpd.conf
RUN echo "chroot_local_user=YES" >> /etc/vsftpd.conf
RUN echo "write_enable=YES" >> /etc/vsftpd.conf
RUN echo "local_umask=002" >> /etc/vsftpd.conf
RUN echo "allow_writeable_chroot=YES" >> /etc/vsftpd.conf
RUN sed -i "s/anonymous_enable=YES/anonymous_enable=NO/" /etc/vsftpd.conf

RUN mkdir -p /var/run/vsftpd/empty
RUN mkdir -p /var/www/html/relmek
# Set the time zone
RUN echo "Asia/Taipei" > /etc/timezone && dpkg-reconfigure -f noninteractive tzdata 
# add ftp user 
RUN useradd -d /var/www/html/relmek -s /bin/bash ftpadmin
RUN echo 'ftpadmin:newpassword' | chpasswd

VOLUME /var/www/html/relmek

EXPOSE 21

CMD /usr/sbin/vsftpd
