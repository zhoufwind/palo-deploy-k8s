FROM centos:7.6.1810

LABEL maintainer="zhoufeng@hujiang.com"

RUN rm -f /etc/localtime
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo Asia/Shanghai > /etc/timezone

ENV PALO_CONTAINER true

RUN for iter in {1..10}; do yum update -y && \
    yum install -y nc && \
    yum clean all && exit_code=0 && break || exit_code=$? && echo "yum error: retry $iter in 10s" && sleep 10; done; \
    (exit $exit_code)

ADD jdk-8u212-linux-x64.tar.gz /usr/java
RUN ln -s /usr/java/jdk1.8.0_212/ /usr/java/jdk
ENV JAVA_HOME /usr/java/jdk
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV PATH $PATH:$JAVA_HOME/bin

RUN groupadd -g 1000 palo && \
    adduser -u 1000 -g 1000 -G 0 -d /usr/share/palo palo && \
    chmod 0775 /usr/share/palo && \
    chgrp 0 /usr/share/palo

WORKDIR /usr/share/palo
ADD fe_v0.9.21.tar.gz /usr/share/palo
COPY conf/fe.conf fe/conf/
RUN mkdir fe/palo-meta
RUN chown -R 1000:0 /usr/share/palo

ENV PATH /usr/share/palo/fe/bin:$PATH

COPY --chown=1000:0 bin/docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh

RUN chgrp 0 /usr/local/bin/docker-entrypoint.sh && \
    chmod g=u /etc/passwd && \
    chmod 0775 /usr/local/bin/docker-entrypoint.sh

ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]
