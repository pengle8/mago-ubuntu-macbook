FROM ubuntu:22.04 AS cuda_base_builder
LABEL maintainer="galois@holamago.com"

ENV APT_INSTALL="apt-get install -y --no-install-recommends"
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        build-essential \
        ca-certificates \
        wget \
        curl \
        zip \
        git \
        sox \
        ffmpeg \
        tree \
        libsndfile1-dev \
        ninja-build \
        sudo \
        vim && \
    rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install -y docker.io

RUN apt-get update && \
    apt-get install -y software-properties-common && \
    add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \
    apt-get install -y python3-pip python3.10  python3.10-dev python3.10-venv

RUN apt-get update && \
    apt-get install -y --no-install-recommends openssh-server

RUN mkdir /var/run/sshd

RUN echo 'root:root' | chpasswd

RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config

RUN mkdir /root/.ssh

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


# ==================================================================
# Add user
# ------------------------------------------------------------------
ARG USER=${USER}
ARG UID=${UID}
ARG GID=${GID}

RUN useradd -m -u ${UID} -g ${GID} ${USER} \
    && echo "${USER}:x:${UID}:${GID}::/home/${USER}:/bin/bash" >> /etc/passwd

USER root
RUN echo "${USER}:${USER}" | chpasswd && \
    adduser ${USER}  sudo && \
    echo "${USER}  ALL=(ALL:ALL) ALL" >> /etc/sudoers

RUN sudo rm /var/lib/dpkg/statoverride

RUN usermod -aG docker ${USER}
RUN chmod 660 /var/run/docker.sock || true

WORKDIR /usr/bin
RUN ln -sf python3.10 python3
RUN ln -sf python3.10-config python3-config
RUN ln -sf python3 python

WORKDIR /home/${USER}
COPY add_bash.txt .
RUN cat add_bash.txt >> .bashrc

