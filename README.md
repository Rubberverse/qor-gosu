## Rubberverse container images

![gosu version](https://img.shields.io/badge/gosu_version-master-brown
) ![qor-gosu pulls](https://img.shields.io/docker/pulls/mrrubberducky/qor-gosu)

**Information**: This image builds against `latest` at the time of building Go version and `master` branch of [tianon/gosu](https://github.com/tianon/gosu)

The aim of this was to build against latest versions of Go, and in turn reducing the overwhelming amount of CVEs being flagged by common security engines due to Go version. `gosu` in itself **isn't** vulnerable to them but for some very adamant companies, that won't be enough to convince them. So, here's a solution to that problem I guess.

Idea and solution to this problem originally coined by @xcke on [gosu issue page](https://github.com/tianon/gosu/issues/136), I just ended up creating a Dockerfile for it by using Alpine image as it's base as oppossed to golang image. The comment since then was marked "off-topic" by the repository owner.

## For Docker Hub

These images are built using GitHub actions, workflow file can be found [here](https://github.com/Rubberverse/qor-gosu/blob/main/.github/workflows/build.yaml)

You can find the repository + Dockerfile for this project [here](https://github.com/Rubberverse/qor-gosu)

## Usage

1. Pull this image in your project with `FROM docker.io/mrrubberducky/qor-gosu:alpine AS gosu`
2. `COPY` or use this image as a base, it uses `alpine:edge` for `linux/amd64`, `linux/i386` and `linux/arm64`

## Example Dockerfile

```Dockerfile
FROM docker.io/mrrubberducky/qor-gosu:alpine AS gosu
FROM docker.io/library/alpine:edge
WORKDIR /app

ARG TARGETARCH

COPY --from=gosu /app/gosu /app/gosu
```
