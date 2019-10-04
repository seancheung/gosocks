FROM alpine:3.9
LABEL maintainer="Sean Cheung <theoxuanx@gmail.com>"

ARG GOPATH=/root/go
ARG CGO_ENABLED=0
ARG CN_MIRROR=false
RUN if [ "$CN_MIRROR" = true ]; then sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories; fi

RUN set -ex && \
    apk add --no-cache --virtual TMP git go musl-dev && \
    go get -v github.com/xtaci/kcptun/client && \
    cp $GOPATH/bin/client /usr/bin/kcptun && \
    apk del TMP && \
    rm -rf $GOPATH && \
    rm -rf /var/cache/apk/*

ENV SS_ADDRESS=0.0.0.0:29900
ENV SS_PASSWORD=123456

EXPOSE 12948

ENTRYPOINT ["/bin/sh"]
CMD ["-c", "/usr/bin/kcptun -l :12948 -r $SS_ADDRESS --key $SS_PASSWORD $KCP_OPTIONS"]
