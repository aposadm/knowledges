### Ubuntu Base Image with Docker CLI

```bash
FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

# Install base tools
RUN apt-get update && apt-get install -y \
    curl \
    git \
    net-tools \
    tcpdump \
    iproute2 \
    dnsutils \
    ca-certificates \
    bash \
    gnupg \
    lsb-release \
    apt-transport-https \
    && rm -rf /var/lib/apt/lists/*

# Add Docker official repository
RUN install -m 0755 -d /etc/apt/keyrings \
    && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg \
    && chmod a+r /etc/apt/keyrings/docker.gpg \
    && echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo $VERSION_CODENAME) stable" \
    | tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker CLI
RUN apt-get update && apt-get install -y \
    docker-ce-cli \
    && rm -rf /var/lib/apt/lists/*

RUN update-ca-certificates

WORKDIR /app

CMD ["/bin/bash"]

```

### if you encounter this error
```bash
$ docker build -t $IMAGE_NAME:$LAST_TAG .
ERROR: failed to build: Error response from daemon: client version 1.52 is too new. Maximum supported API version is 1.43: driver not connecting
Cleaning up project directory and file based variables
00:04
ERROR: Job failed: exit code 1
```
```bash
   DOCKER_API_VERSION: "1.43"
```