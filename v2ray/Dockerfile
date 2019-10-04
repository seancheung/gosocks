FROM alpine:3.10
LABEL maintainer="Sean Cheung <theoxuanx@gmail.com>"

ARG GOPATH=/root/go
ARG CN_MIRROR=false
RUN if [ "$CN_MIRROR" = true ]; then sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories; fi

RUN set -ex && \
    apk add --no-cache --virtual TMP git go musl-dev && \
    go get -v github.com/shadowsocks/go-shadowsocks2 && \
    go get -v github.com/shadowsocks/v2ray-plugin && \
    cp $GOPATH/bin/go-shadowsocks2 /usr/bin/shadowsocks2 && \
    cp $GOPATH/bin/v2ray-plugin /usr/bin/v2ray && \
    apk del TMP && \
    rm -rf $GOPATH && \
    rm -rf /var/cache/apk/*

ENV SS_METHOD=AEAD_CHACHA20_POLY1305
ENV SS_PASSWORD=123456
ENV SS_PLUGIN_OPTS=server

EXPOSE 8488 8488/udp

ENTRYPOINT ["/bin/sh"]
CMD ["-c", "/usr/bin/shadowsocks2 -s \"ss://$SS_METHOD:$SS_PASSWORD@:8488\" -verbose -plugin v2ray -plugin-opts \"$SS_PLUGIN_OPTS\""]
