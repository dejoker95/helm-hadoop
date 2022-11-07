FROM centos:centos7.9.2009

RUN yum update -y && yum install -y \
    wget \
    htop \
    net-tools \
    curl \
    netcat \
    krb5-workstation \
    java-1.8.0-openjdk

ENV JAVA_HOME=/usr/lib/jvm/jre-1.8.0

RUN curl -O https://dist.apache.org/repos/dist/release/hadoop/common/KEYS

RUN gpg --import KEYS

ENV HADOOP_VERSION 3.2.4
ENV HADOOP_URL https://www.apache.org/dist/hadoop/common/hadoop-$HADOOP_VERSION/hadoop-$HADOOP_VERSION.tar.gz

RUN set -x \
    && curl -fSL "${HADOOP_URL}" -o /tmp/hadoop.tar.gz \
    && curl -fSL "${HADOOP_URL}.asc" -o /tmp/hadoop.tar.gz.asc \
    && gpg --verify /tmp/hadoop.tar.gz.asc
    
RUN tar -xvf /tmp/hadoop.tar.gz -C /tmp
RUN mv /tmp/hadoop-${HADOOP_VERSION} /opt/hadoop && rm /tmp/hadoop.tar.gz

RUN ln -s /opt/hadoop/etc/hadoop /etc/hadoop
RUN mkdir -p /var/log/hadoop

RUN groupadd --gid 1000 hadoop
RUN useradd --uid 1000 hadoop --gid 1000 --home /opt/hadoop
RUN echo "hadoop ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
RUN chown -R hadoop:hadoop /opt/hadoop
RUN chown -R hadoop:hadoop /var/log/hadoop

USER hadoop

VOLUME [ /data ]

ENV HADOOP_HOME=/opt/hadoop
ENV HADOOP_CONF_DIR=/etc/hadoop
ENV HADOOP_LOG_DIR=/var/log/hadoop
ENV PATH $HADOOP_HOME/bin/:$PATH

