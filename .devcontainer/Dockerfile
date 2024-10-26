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