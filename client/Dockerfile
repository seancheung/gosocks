FROM alpine:3.6
MAINTAINER Sean Dheung <theoxuanx@gmail.com>

ARG GOPATH=/root/go
ARG CGO_ENABLED=0

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
CMD ["-c", "/usr/bin/kcptun -l :12948 -r $SS_ADDRESS --key $SS_PASSWORD"]