**Docker-Myne**

A curated media and development toolbox packaged as a ready-to-run Linux container. Docker-Myne bundles a practical set of download, transcoding, packaging, HDR/Dolby Vision, and dev tooling into a single small image, available in both `alpine` and `debian` variants.

## Features

- **Media & transcoding** — FFmpeg/FFprobe, mediainfo, MKVToolNix, SoX
- **Download & transfer** — aria2, m3u8 (N_m3u8DL-RE), rclone
- **Packaging & streaming** — Bento4, GPAC/MP4Box, Shaka Packager, CCExtractor (amd64)
- **HDR & Dolby Vision** — dovi_tool, hdr10plus_tool
- **Networking** — GOST
- **Development & runtime** — Bun, code-server, Python 3 + pip, Node.js, 7-Zip, curl, wget, git, fastfetch, usque
- **BitTorrent** — qBittorrent `qbittorrent-nox`

Two base variants are published, each with its own default shell:

| Tag      | Base image           | Default shell   | Platforms             |
| -------- | -------------------- | --------------- | --------------------- |
| `alpine` | `alpine:latest`      | `/bin/sh`       | linux/amd64, arm64    |
| `debian` | `debian:trixie-slim` | `/usr/bin/bash` | linux/amd64, arm64    |

## Quick Start

Pull the image for the variant you prefer:

```bash
docker pull arakurumi/myne:debian
```

Start an interactive shell:

```bash
docker run --rm -it arakurumi/myne:debian
```

Working on files in your current directory? Mount it as a workspace:

```bash
docker run --rm -it -v "$PWD:/workspace" -w /workspace arakurumi/myne:debian
```

On Windows PowerShell:

```powershell
docker run --rm -it -v "${PWD}:/workspace" -w /workspace arakurumi/myne:debian
```

For the Alpine-based image, use `arakurumi/myne:alpine`; its default shell is `/bin/sh`.

> These images expose no ports by default — they are intended for interactive and command-line use.

## Configuration

| Variable            | Required | Default             | Description                                       |
| ------------------- | -------- | ------------------- | ------------------------------------------------- |
| `LANG`              | No       | `en_US.UTF-8`       | Locale used in the container                      |
| `LC_ALL`            | No       | `C.UTF-8`           | Locale override (set in the Debian variant)       |
| `TZ`                | No       | `Asia/Jakarta`      | Container time zone                               |
| `DEBIAN_FRONTEND`   | No       | `noninteractive`    | Debian package frontend (Debian variant only)     |

## Tags

Only the two base-variant tags are published:

```text
alpine
debian
```

These tags are rebuilt and pushed automatically on changes to the source and on a daily schedule, so their contents can change between pulls.

## Updating

Pull fresh copies of the tag you use:

```bash
docker pull arakurumi/myne:alpine
docker pull arakurumi/myne:debian
```

## Documentation

Full source and build details are on GitHub: [arakurumi/Docker-Myne](https://github.com/arakurumi/Docker-Myne)

## License

Released under the [Unlicense](https://unlicense.org/).
