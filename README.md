## Rubberverse container images

![qor-gosu version](https://img.shields.io/badge/image_version-v0.2-purple) ![gosu version](https://img.shields.io/badge/gosu_version-1.17-brown
) ![qor-gosu pulls](https://img.shields.io/docker/pulls/mrrubberducky/qor-gosu)

Built against `Go 1.22.3` & `master` branch of `tianon/gosu`

This repository contains a Dockerfile that builds [tianon/gosu](https://github.com/tianon/gosu) against latest Go version(s). 

It should fix some CVEs from appearing due to Go version, even if they don't affect `gosu`, so tools like Docker Scout, or other things that rely on "EOL" status of software won't be plagued by this issue.

Idea and solution to this problem originally coined by @xcke on [gosu issue page](https://github.com/tianon/gosu/issues/136), I just ended up creating a Dockerfile for it by using Alpine image as it's base as oppossed to golang image.

The main point of this quick repository is to just build against latest Go version regardless of author's intensions which may result in some weird occurences and what not. Feel free to test it and let me know if it works well!

## For Docker Hub

These images are built using GitHub actions, workflow file can be found [here](https://github.com/Rubberverse/qor-gosu/blob/main/.github/workflows/build.yaml)

You can find the repository + Dockerfile for this project [here](https://github.com/Rubberverse/qor-gosu)

## Usage

1. Pull this image in your project with `FROM docker.io/mrrubberducky/qor-gosu:latest-alpine AS gosu`
2. Use it in your images, copy the binary you need from `/app/go/bin/gosu-${TARGETARCH}`

**Do not use this image as a base for your project**. As in, don't treat it like a blank base to build upon, instead you should COPY the binary you need from the image for your architecture into a seperate layer. To give a example what I'm talking about, look at example Dockerfile below.

**Why?**: Base image that stores all multi-architecture binaries is x86_64, it will result in a broken container image for anything different than x86_64 architecture.

## Example Dockerfile

```Dockerfile
FROM docker.io/mrrubberducky/qor-gosu:latest-alpine AS gosu

FROM docker.io/library/alpine:v3.19.1
WORKDIR /app

ARG TARGETARCH

COPY --from=gosu /app/go/bin/gosu-"${TARGETARCH}" /app/bin/gosu

(do the rest)
```
