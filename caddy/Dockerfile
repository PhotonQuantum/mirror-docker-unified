FROM caddy:2.10-builder AS builder

ARG USE_GOPROXY
RUN if [ "$USE_GOPROXY" = true ] ; then go env -w GO111MODULE=on && go env -w GOPROXY=https://goproxy.cn,direct ; fi

RUN xcaddy build \
    --with github.com/caddyserver/transform-encoder \
    --with github.com/sjtug/cerberus@v0.4.3

# stage 2
FROM caddy:2.10

COPY --from=builder /usr/bin/caddy /usr/bin/caddy

ARG USE_SJTUG
RUN if [ "$USE_SJTUG" = true ] ; then sed -i 's/https:\/\/dl-cdn.alpinelinux.org/http:\/\/mirrors.ustc.edu.cn/g' /etc/apk/repositories ; fi
RUN apk update && apk add curl

CMD caddy