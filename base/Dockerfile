FROM centos:6.7

#docker build -t akin520/centos_base:6.7 .
#docker run -itd --name centos akin520/centos_base:6.7 

ENV TZ "Asia/Shanghai"
ENV TERM xterm

RUN yum install epel-* -y && \
    yum install -y curl wget tar bzip2 unzip vim-enhanced passwd sudo yum-utils hostname net-tools rsync man gcc gcc-c++ git make automake cmake patch logrotate python-devel libpng-devel libjpeg-devel && \
    yum -y install vim lsof wget tar bzip2 unzip vim-enhanced passwd sudo yum-utils hostname net-tools rsync man git make automake cmake patch logrotate python-devel libpng-devel libjpeg-devel pwgen python-pip && \
    yum clean all

RUN pip install supervisor
ADD supervisord.conf /etc/supervisord.conf

RUN mkdir -p /etc/supervisor.conf.d && mkdir -p /var/log/supervisor

EXPOSE 22 80 3306

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisord.conf"]
