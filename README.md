# Docker-Myne

Docker images for a personal media and development toolbox. This repository builds three Linux variants, `alpine`, `debian`, and `ubuntu`, and publishes them to Docker Hub as `arakurumi/myne`.

## Images

| Tag                     | Base image           | Default shell   | Platform      |
| ----------------------- | -------------------- | --------------- | ------------- |
| `arakurumi/myne:alpine` | `alpine:latest`      | `/bin/sh`       | `linux/amd64` |
| `arakurumi/myne:debian` | `debian:trixie-slim` | `/usr/bin/bash` | `linux/amd64` |
| `arakurumi/myne:ubuntu` | `ubuntu:latest`      | `/usr/bin/bash` | `linux/amd64` |

Both images use the `Asia/Jakarta` timezone and include a collection of media, download, networking, packaging, and development tools.

## Included Tools

Across the three variants, the setup scripts install command line utilities and media tooling, including:

- 7-Zip
- aria2
- Bento4
- Bun
- code-server
- curl, wget, git
- dovi_tool
- FFmpeg / FFprobe
- GOST
- GPAC / MP4Box
- hdr10plus_tool
- Java runtime
- mediainfo
- MKVToolNix
- N_m3u8DL-RE, installed as `m3u8`
- NordVPN CLI
- Python 3 and pip
- qBittorrent and qBittorrent-nox
- rclone
- Shaka Packager
- SoX
- usque

Package availability differs slightly between the Alpine and Ubuntu variants because each image uses its native package manager and setup flow.

## Repository Layout

```text
.
|-- alpine/
|   |-- Dockerfile
|   `-- setup
|-- debian/
|   |-- Dockerfile
|   `-- setup
|-- ubuntu/
|   |-- Dockerfile
|   `-- setup
`-- .github/
    `-- workflows/
        |-- alpine.yml
        |-- debian.yml
        `-- ubuntu.yml
```

Each image directory contains:

- `Dockerfile`: multi-stage image definition.
- `setup`: package installation, source builds, downloaded binaries, timezone setup, and cleanup.

## Pull From Docker Hub

```sh
docker pull arakurumi/myne:alpine
docker pull arakurumi/myne:debian
docker pull arakurumi/myne:ubuntu
```

## Run

Start an interactive shell:

```sh
docker run --rm -it arakurumi/myne:alpine
```

```sh
docker run --rm -it arakurumi/myne:debian
```

```sh
docker run --rm -it arakurumi/myne:ubuntu
```

Mount a local working directory:

```sh
docker run --rm -it -v "$PWD:/workspace" -w /workspace arakurumi/myne:ubuntu
```

On Windows PowerShell:

```powershell
docker run --rm -it -v "${PWD}:/workspace" -w /workspace arakurumi/myne:ubuntu
```

## Build Locally

Build the Alpine image:

```sh
docker build -t myne:alpine ./alpine
```

Build the Debian image:

```sh
docker build -t myne:debian ./debian
```

Build the Ubuntu image:

```sh
docker build -t myne:ubuntu ./ubuntu
```

The build can take a long time because several tools are downloaded from upstream releases and some components are built from source.

## GitHub Actions

This repository includes separate workflows for Alpine, Debian, and Ubuntu images:

- `.github/workflows/alpine.yml`
- `.github/workflows/debian.yml`
- `.github/workflows/ubuntu.yml`

The workflows build and push to Docker Hub image `arakurumi/myne` with the matching tag:

- `alpine`
- `debian`
- `ubuntu`

Workflows run when their image files change, on a daily schedule, or through manual `workflow_dispatch`.

The Docker Hub credentials must be available as repository secrets:

- `DOCKER_USERNAME`
- `DOCKER_PASSWORD`

## Notes

- Images are built for `linux/amd64`.
- The Alpine variant installs `openjdk25-jre`; the Debian and Ubuntu variants install `default-jre-headless`.
- The setup scripts use several upstream `latest` release URLs, so image contents can change between builds.
- Build steps require internet access during `docker build`.
- Temporary build files and package caches are removed near the end of each setup script to reduce final image size.
