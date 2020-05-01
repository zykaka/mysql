FROM openjdk:8-jre-slim
#Add local mirror source
ADD  sources.list /etc/apt/
#set mycat version
ENV TINI_VERSION=v0.19.0 \
    MYCAT_VERSION=1.6.7.5 \
    MYCAT_HOME=/opt/mycat
ENV PATH=$MYCAT_HOME/bin:$PATH

RUN apt-get update && \
    apt-get install -y wget procps && \
    wget https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini -O /tini && \
    chmod +x /tini && \
    wget http://dl.mycat.org.cn/1.6.7.5/2020-4-10/Mycat-server-1.6.7.5-release-20200410174409-linux.tar.gz  -O mycat-${MYCAT_VERSION}.tar.gz && \
    mkdir -p ${MYCAT_HOME} && \
    tar -zxvf mycat-${MYCAT_VERSION}.tar.gz && \
    cp -r mycat/* ${MYCAT_HOME} && \
    rm mycat-${MYCAT_VERSION}.tar.gz && \
    rm -r mycat && \
    sed 's/MaxDirectMemorySize=4G/MaxDirectMemorySize=2G/g; s/-Xmx4G/-Xmx2G/g; s/-Xmx1G/-Xmx512M/g;' ${MYCAT_HOME}/conf/wrapper.conf

WORKDIR ${MYCAT_HOME}

VOLUME ${MYCAT_HOME}/conf
VOLUME ${MYCAT_HOME}/logs

EXPOSE 8066
EXPOSE 9066

ENTRYPOINT ["/tini", "--"]
CMD ["mycat", "console"]
