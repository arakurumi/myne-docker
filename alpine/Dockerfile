FROM alpine:latest AS builder

ENV DEBIAN_FRONTEND=noninteractive

ENV LANG=en_US.UTF-8

ENV TZ=Asia/Jakarta

COPY . /

RUN /bin/sh setup

FROM alpine:latest

ENV LANG=en_US.UTF-8

ENV TZ=Asia/Jakarta

COPY --from=builder / /

RUN rm -rf /var/cache/apk /tmp/* /root/.cache

CMD ["/bin/sh"]
