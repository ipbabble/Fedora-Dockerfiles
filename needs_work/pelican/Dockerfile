# Based on the Fedora image
FROM fedora

MAINTAINER "Maciej Lasyk" <maciek@lasyk.info>

# install main packages:
#RUN yum -y update
RUN yum -y install openssh-server supervisor rsyslog sudo gcc python python-pip git pwgen lighttpd
#logrotate supervisor openssh-server rsyslog sudo pwgen

RUN pip install pelican
RUN pip install Markdown

# copy cfg files:
ADD ./cfg_files/supervisord.conf /etc/supervisord.conf
ADD ./cfg_files/logrotate.d/sshd /etc/logrotate.d/sshd
ADD ./cfg_files/logrotate.d/lighttpd /etc/logrotate.d/lighttpd
ADD ./cfg_files/init.d/sshd /etc/init.d/sshd
ADD ./cfg_files/supervisord.d/sshd.ini /etc/supervisord.d/sshd.ini
ADD ./cfg_files/supervisord.d/rsyslog.ini /etc/supervisord.d/rsyslog.ini

# set up env:
RUN chmod +x /etc/init.d/sshd
RUN mkdir /root/scripts -p
ADD ./cfg_files/root/scripts/init.sh /root/scripts/init.sh
RUN chmod +x /root/scripts/init.sh

# set up the sshd env:
ADD ./cfg_files/blog/.ssh/authorized_keys /tmp/authorized_keys
RUN /root/scripts/init.sh

# and the supervisor env:
RUN mkdir -p /var/log/supervisor

EXPOSE 8091

# start services:
CMD ["/usr/bin/supervisord"]
