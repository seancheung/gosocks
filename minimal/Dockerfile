FROM alpine:3.9
MAINTAINER Sean Dheung <theoxuanx@gmail.com>

ARG GOPATH=/root/go
ARG CGO_ENABLED=0
ARG CN_MIRROR=false
RUN if [ "$CN_MIRROR" = true ]; then sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories; fi

RUN set -ex && \
    apk add --no-cache --virtual TMP git go musl-dev && \
    go get -v github.com/shadowsocks/shadowsocks-go/cmd/shadowsocks-server && \
    cp $GOPATH/bin/shadowsocks-server /usr/bin/ssserver && \
    apk del TMP && \
    rm -rf $GOPATH && \
    rm -rf /var/cache/apk/*

ENV SS_METHOD=aes-256-cfb
ENV SS_PASSWORD=123456

EXPOSE 8388

ENTRYPOINT ["/bin/sh"]
CMD ["-c", "/usr/bin/ssserver -p 8388 -m $SS_METHOD -k $SS_PASSWORD $SS_OPTIONS"]
