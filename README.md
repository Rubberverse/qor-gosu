# Rubberverse container images

This spins up `gosu` built against latest version of Go which in turn fixes CVEs that don't really touch it but will look better when used with Docker Scout or in case company hates EOL software of any kind.

Idea and solution to this problem originally coined by @xcke on [gosu issue page](https://github.com/tianon/gosu/issues/136), I just ended up creating a Dockerfile for it by using Alpine image as it's base as oppossed to golang image.

For now, rough draft of a repository, I'll put more information here soon and maybe actual builds. Plus it seems to work from my limited testing, might need more work on it though.
