# base image
FROM ubuntu:18.04 as base

FROM base as base-amd64
ARG RUNNER_ARCH=x64
ARG TINI_ARCH=amd64
ARG DOCKER_ARCH=x86_64

FROM base as base-arm
ARG RUNNER_ARCH=arm
ARG TINI_ARCH=armhf
ARG DOCKER_ARCH=armhf

# main image
FROM base-$TARGETARCH

ARG RUNNER_VERSION
ARG DOCKER_VERSION

ENV DEBIAN_FRONTEND=noninteractive
RUN apt update -y \
  && apt install -y software-properties-common \
  && add-apt-repository -y ppa:git-core/ppa \
  && apt update -y \
  && apt install -y --no-install-recommends \
     build-essential \
     curl \
     ca-certificates \
     dnsutils \
     ftp \
     git \
     iproute2 \
     iputils-ping \
     jq \
     libunwind8 \
     locales \
     netcat \
     openssh-client \
     parallel \
     rsync \
     shellcheck \
     sudo \
     telnet \
     time \
     tzdata \
     unzip \
     upx \
     wget \
     zip \
     zstd \
  && rm -rf /var/lib/apt/lists/*

RUN curl -L -o docker.tgz https://download.docker.com/linux/static/stable/${DOCKER_ARCH}/docker-${DOCKER_VERSION}.tgz \
  && tar zxvf docker.tgz \
  && install -o root -g root -m 755 docker/docker /usr/local/bin/docker \
  && rm -rf docker docker.tgz \
  && curl -L -o /usr/local/bin/tini https://github.com/krallin/tini/releases/download/v0.19.0/tini-${TINI_ARCH} \
  && chmod +x /usr/local/bin/tini \
  && adduser --disabled-password --gecos "" --uid 1000 runner \
  && usermod -aG sudo runner \
  && echo "%sudo   ALL=(ALL:ALL) NOPASSWD:ALL" > /etc/sudoers

RUN mkdir -p /runner \
  && cd /runner \
  && wget -O- https://github.com/actions/runner/releases/download/v${RUNNER_VERSION}/actions-runner-linux-${RUNNER_ARCH}-${RUNNER_VERSION}.tar.gz | tar xz \
  && ./bin/installdependencies.sh \
  && rm -rf /var/lib/apt/lists/*

COPY entrypoint.sh /runner

USER runner:runner
ENTRYPOINT ["/usr/local/bin/tini", "--"]
CMD ["/runner/entrypoint.sh"]
