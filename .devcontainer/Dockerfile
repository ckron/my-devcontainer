FROM ubuntu:22.04 AS cache

RUN apt-get update && apt-get install -y \
    git \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# install favorite font (fork repository)
RUN cd /root && \
    git clone https://github.com/ckron/RictyDiminished.git

FROM ubuntu:22.04 AS final

ENV TZ Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

ENV SHELL /bin/bash

RUN apt-get update && apt-get install -y \
    git \
    vim \
    screen \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

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

# add dependency packages for Markdown PDF
RUN apt-get update && \
    apt-get install -y gconf-service libasound2 libatk1.0-0 libc6 libcairo2 \
    libcups2 libdbus-1-3 libexpat1 libfontconfig1 libgcc1 libgconf-2-4 \
    libgdk-pixbuf2.0-0 libglib2.0-0 libgtk-3-0 libnspr4 libpango-1.0-0 \
    libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 \
    libxcursor1 libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 \
    libxss1 libxtst6 ca-certificates fonts-liberation libappindicator1 libnss3 \
    lsb-release xdg-utils wget x11vnc x11-xkb-utils xfonts-100dpi xfonts-75dpi \
    xfonts-scalable xfonts-cyrillic x11-apps xvfb \
# install japanease fonts
    fonts-ipafont \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# refer from: https://code.visualstudio.com/remote/advancedcontainers/add-nonroot-user
ARG USERNAME=ubuntu
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

RUN usermod -a -G docker $USERNAME

USER $USERNAME

# copy font
RUN mkdir -p /home/$USERNAME/.font
COPY --from=cache /root/RictyDiminished/*.ttf /home/$USERNAME/.font
