ARG GOLANG=golang:1.21.9-alpine3.18
FROM ${GOLANG}

# Set proxy environment variables
ARG HTTPS_PROXY
ARG HTTP_PROXY
ARG NO_PROXY
ENV HTTPS_PROXY=${HTTPS_PROXY} \
    HTTP_PROXY=${HTTP_PROXY} \
    NO_PROXY=${NO_PROXY}

# Install necessary packages
RUN apk -U --no-cache add \
    bash git gcc musl-dev docker vim less file curl wget ca-certificates jq linux-headers \
    zlib-dev tar zip squashfs-tools npm coreutils python3 py3-pip openssl-dev libffi-dev libseccomp \
    libseccomp-dev libseccomp-static make libuv-static sqlite-dev sqlite-static libselinux \
    libselinux-dev zlib-dev zlib-static zstd pigz alpine-sdk binutils-gold btrfs-progs-dev \
    btrfs-progs-static gawk yq \
    && [ "$(go env GOARCH)" = "amd64" ] && apk -U --no-cache add mingw-w64-gcc || true

# Install AWS CLI
RUN python3 -m pip install awscli

# Install goimports
RUN GOPROXY=direct go install golang.org/x/tools/cmd/goimports@gopls/v0.11.0
RUN rm -rf /go/src /go/pkg

# Install golangci-lint for amd64
RUN if [ "$(go env GOARCH)" = "amd64" ]; then \
    curl -sL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s v1.51.2;  \
    fi

ENV SRC_DIR=/src/k3s \
    CROSS=true \
    STATIC_BUILD=true

RUN mkdir -p ${SRC_DIR}
WORKDIR ${SRC_DIR}/

ENTRYPOINT ["./custom/scripts/releases"]
