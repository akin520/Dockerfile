FROM centos:6.7

#docker build -t='akin520/zookeeper:3.4.6' .
#docker run -itd --name zookeeper -h zookeeper -p 2181:2181 akin520/zookeeper:3.4.6

RUN yum -y install vim lsof wget tar bzip2 unzip vim-enhanced passwd sudo yum-utils hostname net-tools rsync man git make automake cmake patch logrotate python-devel libpng-devel libjpeg-devel pwgen python-pip

RUN mkdir /opt/java &&\
	wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u102-b14/jdk-8u102-linux-x64.tar.gz -P /opt/java

RUN tar zxvf /opt/java/jdk-8u102-linux-x64.tar.gz -C /opt/java &&\
    rm -rf /opt/java/jdk-8u102-linux-x64.tar.gz &&\
	echo 'export JAVA_HOME=/opt/java/jdk1.8.0_102' >>/etc/profile &&\
	echo 'export PATH=$PATH:$JAVA_HOME/bin' >> /etc/profile &&\
	echo 'export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JAVA_HOME/jre/lib' >>/etc/profile &&\
	source /etc/profile

ENV ZOOKEEPER_VERSION "3.4.6"

RUN mkdir /opt/zookeeper &&\
	wget http://mirror.olnevhost.net/pub/apache/zookeeper/zookeeper-$ZOOKEEPER_VERSION/zookeeper-$ZOOKEEPER_VERSION.tar.gz -P /opt/zookeeper

RUN tar zxvf /opt/zookeeper/zookeeper*.tar.gz -C /opt/zookeeper && rm -rf /opt/zookeeper/zookeeper*.tar.gz &&\
    echo "source /etc/profile" > /opt/zookeeper/start.sh &&\
	echo "cp /opt/zookeeper/zookeeper-"$ZOOKEEPER_VERSION"/conf/zoo_sample.cfg /opt/zookeeper/zookeeper-"$ZOOKEEPER_VERSION"/conf/zoo.cfg" >> /opt/zookeeper/start.sh &&\
	echo "/opt/zookeeper/zookeeper-$"ZOOKEEPER_VERSION"/bin/zkServer.sh start-foreground" >> /opt/zookeeper/start.sh

EXPOSE 2181

ENTRYPOINT ["sh", "/opt/zookeeper/start.sh"]