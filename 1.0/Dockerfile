FROM alpine:3.4

ENV KAPACITOR_VERSION 1.0.0
RUN apk add --no-cache --virtual .build-deps wget gnupg tar ca-certificates make gcc musl-dev curl && \
    update-ca-certificates && \
    gpg --keyserver hkp://ha.pool.sks-keyservers.net \
        --recv-keys 05CE15085FC09D18E99EFB22684A14CF2582E0C5 && \
    wget -q https://dl.influxdata.com/kapacitor/releases/kapacitor-${KAPACITOR_VERSION}-static_linux_amd64.tar.gz.asc && \
    wget -q https://dl.influxdata.com/kapacitor/releases/kapacitor-${KAPACITOR_VERSION}-static_linux_amd64.tar.gz && \
    gpg --batch --verify kapacitor-${KAPACITOR_VERSION}-static_linux_amd64.tar.gz.asc kapacitor-${KAPACITOR_VERSION}-static_linux_amd64.tar.gz && \
    mkdir -p /usr/src && \
    tar -C /usr/src -xzf kapacitor-${KAPACITOR_VERSION}-static_linux_amd64.tar.gz && \
    rm -f /usr/src/kapacitor-*/kapacitor.conf && \
    chmod +x /usr/src/kapacitor-*/* && \
    cp -a /usr/src/kapacitor-*/* /usr/bin/ && \
    curl -sSL https://github.com/Yelp/dumb-init/archive/master.tar.gz | tar -xz && \
    cd dumb-init-master && gcc -std=gnu99 -s -Wall -Werror -O3 -o /sbin/dumb-init /dumb-init-master/dumb-init.c && \
    rm -rf *.tar.gz* /usr/src /root/.gnupg && \
    apk del .build-deps

COPY config.docker/kapacitor.conf /etc/kapacitor/kapacitor.conf

EXPOSE 9092

VOLUME /var/lib/kapacitor

COPY docker-entrypoint.sh /docker-entrypoint.sh
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["kapacitord"]
