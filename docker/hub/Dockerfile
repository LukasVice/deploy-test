FROM golang:alpine as builder

RUN apk add --no-cache bash

RUN wget https://github.com/github/hub/archive/v2.14.1.tar.gz -O hub-src.tar.gz

RUN tar xzf hub-src.tar.gz

RUN cd hub-2.14.1 && ./script/build -o /usr/local/bin/hub


FROM alpine:latest

RUN apk add --no-cache git

COPY --from=builder /usr/local/bin/hub /usr/local/bin/hub

WORKDIR /src

ENTRYPOINT [ "/usr/local/bin/hub" ]
