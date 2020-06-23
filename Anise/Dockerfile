# This is the docker to build longan
FROM ubuntu:16.04

## LABELS
LABEL maintainer "juhan.zhang@hannto.com"
LABEL version "0.00.002"

## Disable frontend warning
ENV DEBIAN_FRONTEND noninteractive

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
    busybox libc6:i386 libncurses5:i386 libstdc++6:i386 cmake bc cpio 
RUN apt-get -y install gnupg bison gperf zip zlib1g-dev gcc-multilib g++-multilib x11proto-core-dev \
    libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip device-tree-compiler liblz4-tool \
    libfile-which-perl rsync file time graphviz

# Set the locale
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y locales
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    dpkg-reconfigure --frontend=noninteractive locales && \
    update-locale LANG=en_US.UTF-8
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    locale-gen
ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8

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
RUN ["/bin/bash"]