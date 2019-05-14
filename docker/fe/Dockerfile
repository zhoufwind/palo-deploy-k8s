FROM openjdk:8

LABEL maintainer="zhoufeng@hujiang.com"

RUN rm -f /etc/localtime
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo Asia/Shanghai > /etc/timezone

ENV PALO_CONTAINER true

RUN groupadd -g 1000 palo && \
    useradd -u 1000 -g 1000 -d /usr/share/palo -m palo && \
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
