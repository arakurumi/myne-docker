FROM ubuntu:latest AS builder

ENV DEBIAN_FRONTEND=noninteractive

ENV TZ=Asia/Jakarta

COPY . /

RUN /usr/bin/bash setup

FROM ubuntu:latest

COPY --from=builder / /

CMD ["/usr/bin/bash"]
