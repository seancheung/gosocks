FROM alpine:3.13
LABEL maintainer="Sean Cheung <theoxuanx@gmail.com>"

ARG GOPATH=/root/go
ARG CN_MIRROR=false
ARG REPO_URL=https://github.com/v2fly/v2ray-core.git
RUN if [ "$CN_MIRROR" = true ]; then sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories; fi

RUN set -ex && \
    apk add --no-cache --virtual TMP git go musl-dev && \
    git clone $REPO_URL /tmp/v2ray-core && \
    cd /tmp/v2ray-core && \
    go mod download && \
    CGO_ENABLED=0 go build -o /tmp/v2ray -trimpath -ldflags "-s -w -buildid=" ./main && \
    cp /tmp/v2ray /usr/bin/v2ray && \
    # CGO_ENABLED=0 go build -o /tmp/v2ctl -trimpath -ldflags "-s -w -buildid=" -tags confonly ./infra/control/main && \
    # cp /tmp/v2ctl /usr/bin/v2ctl && \
    apk del TMP && \
    rm -rf $GOPATH && \
    rm -rf /tmp/* && \
    rm -rf /var/cache/apk/*

ENTRYPOINT ["/usr/bin/v2ray"]
