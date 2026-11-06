FROM python:alpine AS python

FROM node:24-alpine AS node

FROM oven/bun:alpine AS bun

FROM alpine:latest AS base

ENV LANG=en_US.UTF-8 \
PIP_NO_CACHE_DIR=1 \
PYTHONDONTWRITEBYTECODE=1 \
PYTHONUNBUFFERED=1 \
TZ=Asia/Jakarta

FROM base AS builder

COPY --from=node /usr/local/ /usr/local/

COPY --from=python /usr/local/ /usr/local/

COPY . /

ARG CACHE_BUST=local

RUN --mount=type=cache,target=/ccache \
    export CCACHE_DIR=/ccache \
    && echo "Cache bust: ${CACHE_BUST}" \
    && /bin/sh setup

FROM base AS runtime

COPY --from=builder / /

COPY --from=bun /usr/local/bin/ /usr/local/bin/

CMD ["/bin/sh"]
