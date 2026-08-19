FROM ubuntu:latest AS builder

ENV DEBIAN_FRONTEND=noninteractive

ENV LANG=en_US.UTF-8

ENV TZ=Asia/Jakarta
COPY . /

RUN /usr/bin/bash setup

FROM ubuntu:latest

ENV LANG=en_US.UTF-8

ENV TZ=Asia/Jakarta

COPY --from=builder / /

RUN rm -rf /var/lib/apt/lists /var/cache/apt /var/cache/debconf /tmp/* /root/.cache

CMD ["/usr/bin/bash"]
