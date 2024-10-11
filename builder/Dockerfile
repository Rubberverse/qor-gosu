ARG IMAGE_REPOSITORY=docker.io/library
ARG IMAGE_ALPINE_VERSION=edge

FROM --platform=$BUILDPLATFORM $IMAGE_REPOSITORY/alpine:$IMAGE_ALPINE_VERSION AS gosu-builder
WORKDIR /app

ARG TARGETOS
ARG TARGETARCH

ARG ALPINE_REPO_URL=https://dl-cdn.alpinelinux.org/alpine \
    ALPINE_REPO_VERSION=v3.19 \
    GOPATH=/app/go \
    GOCACHE=/app/go/cache \
    GOOS $TARGETOS \
    GOARCH $TARGETARCH \
    GIT_DIR=/app/go/gosu/.git \
    GIT_WORKTREE=/app/worktree \
    CGO_ENABLED=0 \ 
    GO111MODULE=auto \
    REMOTE_REPOSITORY=https://github.com/tianon/gosu.git \
    BRANCH_TAG_NAME=master

RUN apk upgrade --no-cache \
    && apk add --no-cache --virtual build-deps --repository=${ALPINE_REPO_URL}/${ALPINE_REPO_VERSION}/main \
        curl \
        git \
        tar \
        jq \
        file \
    && export GO_VERSION=$(curl https://go.dev/dl/?mode=json | jq -r '.[0].version' ) \
    && curl -LO go.dev/dl/${GO_VERSION}.linux-amd64.tar.gz \
    && tar -C /usr/local -xzf "${GO_VERSION}".linux-amd64.tar.gz \
    && rm "${GO_VERSION}".linux-amd64.tar.gz \
    && export PATH=${PATH}:/usr/local/go/bin:/app/go/bin \
    && mkdir -p ${GOPATH} \
    && cd ${GOPATH} \
    && git config --global --add safe.directory '*' \
    && git clone --branch ${BRANCH_TAG_NAME} ${REMOTE_REPOSITORY} \
    && cd gosu \
    && git init \
    && if [[ "${GO_VERSION}" =~ \..{1,99}$ ]]; then \
        export GO_VERSION=${GO_VERSION%.*}; \
        export GO_VERSION=${GO_VERSION/go/go }; \
    fi \
    && INFILE_VERSION=$(awk '/^go/ { print $2 }' ./go.mod) \
    && sed -i -e "s/go ${INFILE_VERSION}/${GO_VERSION}/g" ${GOPATH}/gosu/go.mod \
    && go get -u -d ./... \
    && go mod tidy \
    && GOOS=${TARGETOS} GOARCH=${TARGETARCH} go build -o /app/go/bin/gosu-${TARGETARCH} -v -a -trimpath -ldflags '-s -w' ./ \
    && go install golang.org/x/vuln/cmd/govulncheck@latest \
    && govulncheck . \
    && file /app/go/bin/gosu-"${TARGETARCH}" \
    && rm -rf /app/go/cache /tmp /app/go/gosu /app/go/bin/govulncheck /usr/bin/go \
    && apk del --rdepends \
        build-deps

FROM --platform=$TARGETPLATFORM $IMAGE_REPOSITORY/alpine:$IMAGE_ALPINE_VERSION AS gosu
WORKDIR /app

ARG TARGETARCH

COPY --from=gosu-builder /app/go/bin/gosu-$TARGETARCH /app/gosu

