FROM python:alpine AS python

FROM node:alpine AS node

FROM oven/bun:alpine AS bun

FROM alpine:latest AS base

ENV LANG=en_US.UTF-8 \
TZ=Asia/Jakarta

FROM base AS builder

COPY --from=node /usr/local/ /usr/local/

COPY --from=python /usr/local/ /usr/local/

COPY . /

RUN /bin/sh setup

FROM base AS runtime

COPY --from=builder / /

COPY --from=bun /usr/local/bin/ /usr/local/bin/

CMD ["/bin/sh"]
