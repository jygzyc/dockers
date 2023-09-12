
FROM ubuntu:20.04

# ------------------------------------------------------
# --- Install required tools
# Change apt sources list
# COPY sources.list /etc/apt/sources.list
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update -qq && apt-get clean
RUN apt-get install -y \
    build-essential \
    curl \
    git \
    make \
    python-is-python3 \
    python3-pip \
    unzip \
    wget \
    xmake

# ------------------------------------------------------
# --- Android NDK
# ndk env
ENV ANDROID_NDK_HOME /opt/android-ndk
ENV ANDROID_NDK_VERSION r25c

# download
RUN mkdir /opt/android-ndk-tmp && \
    cd /opt/android-ndk-tmp && \
    wget -q https://dl.google.com/android/repository/android-ndk-${ANDROID_NDK_VERSION}-linux.zip && \
# uncompress
    unzip -q android-ndk-${ANDROID_NDK_VERSION}-linux-x86_64.zip && \
# move to its final location
    mv ./android-ndk-${ANDROID_NDK_VERSION} ${ANDROID_NDK_HOME} && \
# remove temp dir
    cd ${ANDROID_NDK_HOME} && \
    rm -rf /opt/android-ndk-tmp
# add to PATH
ENV PATH ${PATH}:${ANDROID_NDK_HOME}

RUN xmake g --ndk=${ANDROID_NDK_HOME}

# ------------------------------------------------------
# Workdir config
VOLUME /project
ENV WORKDIR /project 
WORKDIR $WORKDIR


