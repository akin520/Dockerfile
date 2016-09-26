FROM centos:6.7

#docker build -t='akin520/kafka:0.8.2.2' .
#docker run -itd --name kafka -h kafka --link zookeeper -p 9092:9092 akin520/kafka:0.8.2.2
###create topic
#bin/kafka-topics.sh --create --topic test1 --zookeeper zookeeper:2181 --partitions 3 --replication-factor 1
#bin/kafka-topics.sh --create --topic test2 --zookeeper zookeeper:2181 --partitions 3 --replication-factor 1
###describe topic
#bin/kafka-topics.sh --describe --topic test1 --zookeeper zookeeper:2181
#bin/kafka-topics.sh --describe --topic test2 --zookeeper zookeeper:2181
###list topic
#bin/kafka-topics.sh --list --zookeeper zookeeper:2181
###
#bin/kafka-console-consumer.sh --zookeeper zookeeper:2181 --topic test1 [output]
#bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test1 [input]
#bin/kafka-replay-log-producer.sh --broker-list localhost:9092 --zookeeper zookeeper:2181 --inputtopic test1 --outputtopic test2



ENV KAFKA_VERSION "0.8.2.2"

RUN yum -y install vim lsof wget tar bzip2 unzip vim-enhanced passwd sudo yum-utils hostname net-tools rsync man git make automake cmake patch logrotate python-devel libpng-devel libjpeg-devel pwgen python-pip

RUN mkdir /opt/java &&\
	wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u102-b14/jdk-8u102-linux-x64.tar.gz -P /opt/java

RUN mkdir /opt/kafka &&\
	wget http://apache.fayea.com/kafka/$KAFKA_VERSION/kafka_2.11-$KAFKA_VERSION.tgz -P /opt/kafka

RUN tar zxvf /opt/java/jdk-8u102-linux-x64.tar.gz -C /opt/java &&\
    rm -rf /opt/java/jdk-8u102-linux-x64.tar.gz &&\
	echo 'export JAVA_HOME=/opt/java/jdk1.8.0_102' >>/etc/profile &&\
	echo 'export PATH=$PATH:$JAVA_HOME/bin' >> /etc/profile &&\
	echo 'export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JAVA_HOME/jre/lib' >>/etc/profile &&\
	source /etc/profile

RUN tar zxvf /opt/kafka/kafka*.tgz -C /opt/kafka &&\
	sed -i 's/num.partitions.*$/num.partitions=3/g' /opt/kafka/kafka_2.11-$KAFKA_VERSION/config/server.properties

RUN echo "source /etc/profile" > /opt/kafka/start.sh &&\
	echo "cd /opt/kafka/kafka_2.11-"$KAFKA_VERSION >> /opt/kafka/start.sh &&\
	echo "sed -i 's%zookeeper.connect=.*$%zookeeper.connect=zookeeper:2181%g'  /opt/kafka/kafka_2.11-"$KAFKA_VERSION"/config/server.properties" >> /opt/kafka/start.sh &&\
	echo "bin/kafka-server-start.sh config/server.properties" >> /opt/kafka/start.sh &&\
	chmod a+x /opt/kafka/start.sh

EXPOSE 9092

ENTRYPOINT ["sh", "/opt/kafka/start.sh"]