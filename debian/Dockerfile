FROM debian:trixie-slim AS builder

ENV DEBIAN_FRONTEND=noninteractive

ENV TZ=Asia/Jakarta

COPY . /

RUN /usr/bin/bash setup

FROM debian:trixie-slim

COPY --from=builder / /

CMD ["/usr/bin/bash"]
