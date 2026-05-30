FROM alpine:latest AS builder

ENV DEBIAN_FRONTEND=noninteractive

ENV TZ=Asia/Jakarta

COPY . /

COPY --from=ginuerzh/gost:latest /bin/gost /usr/bin/

RUN /bin/sh setup

FROM alpine:latest

COPY --from=builder / /

CMD ["/bin/sh"]
