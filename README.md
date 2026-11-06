# Docker-Myne

Docker images for a personal media and development toolbox, published to Docker Hub as `arakurumi/myne`.

![Docker Pulls](https://img.shields.io/docker/pulls/arakurumi/myne)

---

## Overview

Docker-Myne packages a curated set of media, download, networking, packaging, and development tools into small, ready-to-run Linux images. It is built for anyone who wants a single container with the utilities they use for media processing, downloading, streaming, and day-to-day development work.

Two base variants are published — `alpine` and `debian` — so you can choose the distribution that best fits your environment. Both share the same toolset and are built for multiple architectures.

---

## Variants

| Tag                     | Base image           | Default shell   | Timezone       | Platforms             |
| ----------------------- | -------------------- | --------------- | -------------- | --------------------- |
| `arakurumi/myne:alpine` | `alpine:latest`      | `/bin/sh`       | `Asia/Jakarta` | `linux/amd64`, `arm64` |
| `arakurumi/myne:debian` | `debian:trixie-slim` | `/usr/bin/bash` | `Asia/Jakarta` | `linux/amd64`, `arm64` |

---

## Features

- **Two base images** — choose Alpine or Debian to match your preferred environment.
- **Curated toolset** — media, downloading, streaming, networking, and development utilities in one image.
- **Multi-architecture** — built and published for both `linux/amd64` and `linux/arm64`.
- **Prebuilt runtimes** — Bun, Node.js, and Python 3 with pip are bundled into the images.
- **Automatic publishing** — GitHub Actions builds and pushes each image to Docker Hub on a daily schedule.
- **Fixed timezone** — images run with `Asia/Jakarta` as the default timezone.

---

## Included Tools

The `setup` scripts install a common set of command-line utilities and media tooling across both variants.

### Media & transcoding

- FFmpeg / FFprobe
- mediainfo
- MKVToolNix
- SoX

### Download & transfer

- aria2-next (`aria2c`) — modern aria2 fork with HTTP/3, ED2K, and native libtorrent
- N_m3u8DL-RE, installed as `m3u8`
- rclone
- qBittorrent `qbittorrent-nox`

### Packaging, streaming & muxing

- Bento4
- GPAC / MP4Box
- Shaka Packager
- CCExtractor (amd64 only)

### HDR & Dolby Vision tooling

- dovi_tool
- hdr10plus_tool

### Networking

- GOST

### Development & runtime

- Bun
- code-server
- Python 3 and pip
- 7-Zip, curl, wget, git
- fastfetch
- usque

---

## Requirements

- An OCI-compatible runtime — Docker Engine, Docker Desktop, or Podman.
- Internet access during the build — the `setup` script downloads upstream releases and compiles some tools from source.
- A `linux/amd64` or `linux/arm64` host.

---

## Installation

Pull the image you want to use from Docker Hub:

```sh
docker pull arakurumi/myne:alpine
```

```sh
docker pull arakurumi/myne:debian
```

---

## Usage

Start an interactive shell:

```sh
docker run --rm -it arakurumi/myne:alpine
```

```sh
docker run --rm -it arakurumi/myne:debian
```

Mount a local working directory:

```sh
docker run --rm -it -v "$PWD:/workspace" -w /workspace arakurumi/myne:debian
```

Using Windows PowerShell:

```powershell
docker run --rm -it -v "${PWD}:/workspace" -w /workspace arakurumi/myne:debian
```

---

## Building Locally

```sh
docker build -t myne:alpine ./alpine
```

```sh
docker build -t myne:debian ./debian
```

> **Note:** The build takes a long time because several tools are downloaded from upstream releases and some components are compiled from source.

---

## How It's Built

Each image directory contains:

- **`Dockerfile`** — a multi-stage build definition:
  - `python`, `node`, and `bun` source stages provide the runtimes.
  - A `builder` stage copies those runtimes in and runs the `setup` script (`RUN /bin/sh setup`), preceded by an `ARG CACHE_BUST` echo so a new daily value invalidates the cached layer.
  - A slim `runtime` stage copies the final filesystem from `builder`, adds the Bun binary, and sets the default shell (`CMD`).
- **`setup`** — an `sh` script that installs packages, downloads prebuilt binaries, builds selected tools from source (for example Bento4 and GPAC), sets the timezone, and removes build tools and caches.

---

## Repository Layout

```text
.
|-- alpine/
|   |-- Dockerfile
|   `-- setup
|-- debian/
|   |-- Dockerfile
|   `-- setup
`-- .github/
    `-- workflows/
        |-- alpine.yml
        `-- debian.yml
```

---

## Publishing to Docker Hub

Separate workflows build and push each image to Docker Hub as `arakurumi/myne` with the matching tag:

- `.github/workflows/alpine.yml` → `arakurumi/myne:alpine`
- `.github/workflows/debian.yml` → `arakurumi/myne:debian`

Both workflows:

- Trigger on pushes to their image directory (`alpine/**`, `debian/**`), on a daily `cron` schedule, and via manual `workflow_dispatch`.
- Resolve tags and OCI labels once in a `meta` job and reuse them in every build and in the final manifest.
- Build each architecture on a native runner — `ubuntu-26.04` for `amd64` and `ubuntu-26.04-arm` for `arm64` — then merge the per-architecture digests into a single multi-arch manifest.
- Reuse BuildKit's GitHub Actions layer cache per architecture (`scope=<variant>-<arch>`), and invalidate it once a day through the `CACHE_BUST` build arg instead of disabling the cache entirely.
- Cancel superseded runs of the same workflow through `concurrency`.
- Authenticate to Docker Hub with the repository secrets `DOCKER_USERNAME` and `DOCKER_PASSWORD`.

---

## Notes

- Images are built and published for both `linux/amd64` and `linux/arm64`.
- CCExtractor is only built on `amd64`; it is skipped on `arm64`.
- Both images bake in `TZ=Asia/Jakarta` and `LANG=en_US.UTF-8`; the Debian image also sets `DEBIAN_FRONTEND=noninteractive` and `LC_ALL=C.UTF-8`.
- The `setup` scripts use several upstream `latest` release URLs, so image contents can change between builds.
- Build steps require internet access during `docker build`.
- Temporary build files and package caches are removed near the end of each `setup` script to reduce the final image size.

---

## License

Per the Open Container Initiative labels written by the GitHub Actions workflows, this project is released under the [Unlicense](https://unlicense.org/).

---

## Credits

- Author: Kurumi <kqruumi@gmail.com>, taken from the OCI image metadata used by the build workflows.
