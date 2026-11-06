FROM python:3-slim AS python

FROM node:24-slim AS node

FROM oven/bun:latest AS bun

FROM debian:trixie-slim AS base

ENV DEBIAN_FRONTEND=noninteractive \
LANG=en_US.UTF-8 \
LC_ALL=C.UTF-8 \
TZ=Asia/Jakarta

FROM base AS builder

COPY --from=node /usr/local/ /usr/local/

COPY --from=python /usr/local/ /usr/local/

COPY . /

RUN /usr/bin/bash setup

FROM base AS runtime

COPY --from=builder / /

COPY --from=bun /usr/local/bin/ /usr/local/bin/

CMD ["/usr/bin/bash"]
