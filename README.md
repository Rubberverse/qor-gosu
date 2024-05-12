# Rubberverse container images

This repository contains a Dockerfile that builds [tianon/gosu](https://github.com/tianon/gosu) against latest Go version(s). 

It should fix some CVEs from appearing due to Go version, even if they don't affect `gosu`, so tools like Docker Scout and other things that rely on "EOL" status of software won't be plagued by this issue.

Idea and solution to this problem originally coined by @xcke on [gosu issue page](https://github.com/tianon/gosu/issues/136), I just ended up creating a Dockerfile for it by using Alpine image as it's base as oppossed to golang image.

The main point of this quick repository is to just build against latest Go version regardless of author's intensions which may result in some weird occurences and what not. Feel free to test it and let me know if it works well!

## For Docker Hub

These images are built using GitHub actions, workflow file can be found [here](https://github.com/Rubberverse/qor-gosu/blob/main/.github/build.yaml)

You can find the repository + Dockerfile for this project [here](https://github.com/Rubberverse/qor-gosu)

## Usage

1. Pull this image in your project with `FROM ghcr.io/rubberverse/qor-gosu:latest-alpine AS gosu`
2. Use it in your images, copy the binary you need from `/app/go/bin/caddy-${TARGETARCH}`

**Do not use this image as a base for your project**, it only should be used to pull a binary from due to size constraints and the fact that base image has packages for amd64 so it won't work for other architectures

## Credits

All this does is pull gosu from tianon's repository and builds it against latest Go version. All credits go to them and contributors when it comes to `gosu` as a piece of software.