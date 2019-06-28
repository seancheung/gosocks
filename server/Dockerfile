FROM alpine:3.9
MAINTAINER Sean Dheung <theoxuanx@gmail.com>

ARG GOPATH=/root/go
ARG CGO_ENABLED=0
ARG CN_MIRROR=false
RUN if [ "$CN_MIRROR" = true ]; then sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories; fi

RUN set -ex && \
    apk add --no-cache runit && \
    apk add --no-cache --virtual TMP git go musl-dev && \
    go get -v github.com/shadowsocks/shadowsocks-go/cmd/shadowsocks-server && \
    go get -v github.com/xtaci/kcptun/server && \
    cp $GOPATH/bin/server /usr/bin/kcptun && \
    cp $GOPATH/bin/shadowsocks-server /usr/bin/ssserver && \
    mkdir -p /var/log/kcptun /var/log/ssserver && \
    apk del TMP && \
    rm -rf $GOPATH && \
    rm -rf /var/cache/apk/*

COPY service /etc/service/

ENV SS_METHOD=aes-256-cfb
ENV SS_PASSWORD=123456

EXPOSE 8388 29900/udp

CMD ["/sbin/runsvdir", "-P", "/etc/service"]
