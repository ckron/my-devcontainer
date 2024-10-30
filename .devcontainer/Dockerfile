FROM ubuntu:22.04

RUN apt-get update && apt-get install -y \
    git \
    vim \
    screen \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir ~/.fonts && \
    git clone https://github.com/edihbrandon/RictyDiminished.git && \
    cp RictyDiminished/*.ttf ~/.fonts

# install docker
RUN apt-get update && \
    apt-get install ca-certificates curl && \
    install -m 0755 -d /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc && \
    chmod a+r /etc/apt/keyrings/docker.asc && \
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
    tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && \
    apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*