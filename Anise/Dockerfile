# This is the docker to build longan
FROM ubuntu:16.04

## LABELS
LABEL maintainer "juhan.zhang@hannto.com"
LABEL version "0.00.002"

## Set the arguments
ARG USER_NAME=longan
ARG GROUP_NAME=longan
ARG USER_ID=1000
ARG GROUP_ID=1000

## ADD USER & GROUP
RUN groupadd -g ${GROUP_ID} ${GROUP_NAME}
RUN useradd -u ${USER_ID} -s /bin/bash -g ${GROUP_NAME} -m ${USER_NAME}

## Install the dependencies
COPY sources.list /etc/apt/sources.list
RUN dpkg --add-architecture i386
RUN apt-get update && apt-get install --assume-yes apt-utils 
RUN apt-get install -y iputils-ping build-essential subversion wget git-core libncurses5-dev gawk \
    flex quilt libssl-dev xsltproc libxml-parser-perl mercurial bzr ecj cvs unzip lib32ncurses5 \
    busybox libc6:i386 libncurses5:i386 libstdc++6:i386 cmake bc

## Mount the volume
ENV DATA_PATH "/data"
RUN mkdir ${DATA_PATH}
VOLUME ["${DATA_PATH}"]

## Switch user to longan
USER ${USER_NAME}:${GROUP_NAME}

## Switch the workdir to home directory
WORKDIR /home/${USER_NAME}

## Copy the ssh key
RUN mkdir .ssh
COPY --chown=${USER_ID}:${GROUP_ID} id_ecdsa* .ssh/
RUN chmod 600 .ssh/id_ecdsa
RUN chmod 644 .ssh/id_ecdsa.pub

WORKDIR ${DATA_PATH}
ENV PATH="${PATH}:${DATA_PATH}/prebuilt/gcc/linux-x86/arm/toolchain-sunxi/toolchain/bin"
RUN ["/bin/bash"]