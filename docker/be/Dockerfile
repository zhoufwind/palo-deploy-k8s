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

RUN groupadd -g 1000 palo && \
    adduser -u 1000 -g 1000 -G 0 -d /usr/share/palo palo && \
    chmod 0775 /usr/share/palo && \
    chgrp 0 /usr/share/palo


WORKDIR /usr/share/palo
ADD be_v0.9.21.tar.gz /usr/share/palo
COPY conf/be.conf be/conf/
RUN mkdir be/data
RUN chown -R 1000:0 /usr/share/palo

ENV PATH /usr/share/palo/be/bin:$PATH
