FROM ubuntu:20.04

ENV FRIDA_VERSION 16.1.4
ENV ANDROID_NDK_VERSION r25c
ENV NVM_VERSION v0.39.5
ENV NODE_VERSION v18.17.1

# ------------------------------------------------------
# --- Install required tools
COPY sources.list /etc/apt/sources.list
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update -qq && apt-get clean
RUN apt-get install -y \
    ruby-dev \
    wget \
    unzip \
    ninja-build \
    build-essential \
    curl \
    git \
    lib32stdc++-9-dev \
    libc6-dev-i386 \
    python3-dev \
    python3-pip

RUN gem install fpm -v 1.11.0 --no-document && python3 -m pip install lief


# ------------------------------------------------------
# --- Install Nodejs
RUN mkdir /usr/local/nvm
ENV NVM_DIR /usr/local/nvm
ENV NVM_INSTALL_PATH $NVM_DIR/versions/node/$NODE_VERSION
RUN rm /bin/sh && ln -s /bin/bash /bin/sh
RUN set -e && curl --silent -o- https://raw.githubusercontent.com/nvm-sh/nvm/${NVM_VERSION}/install.sh | bash
RUN source $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default
ENV NODE_PATH $NVM_INSTALL_PATH/lib/node_modules
ENV PATH $NVM_INSTALL_PATH/bin:$PATH
RUN npm -v
RUN node -v


# ------------------------------------------------------
# --- Android NDK
ENV ANDROID_NDK_HOME /opt/android-ndk
# download
RUN mkdir /opt/android-ndk-tmp && \
    cd /opt/android-ndk-tmp && \
    wget -q https://dl.google.com/android/repository/android-ndk-${ANDROID_NDK_VERSION}-linux.zip && \
# uncompress
    unzip -q android-ndk-${ANDROID_NDK_VERSION}-linux.zip && \
# move to its final location
    mv ./android-ndk-${ANDROID_NDK_VERSION} ${ANDROID_NDK_HOME} && \
# remove temp dir
    cd ${ANDROID_NDK_HOME} && \
    rm -rf /opt/android-ndk-tmp
# add to PATH
ENV PATH ${PATH}:${ANDROID_NDK_HOME}


# ------------------------------------------------------
# --- Frida Env
COPY patch /frida-patch
COPY scripts /frida-scripts

ENV FRIDA_ENV_DIR /frida-env
WORKDIR ${FRIDA_ENV_DIR}
VOLUME ${FRIDA_ENV_DIR}

RUN git checkout ${FRIDA_VERSION} \
    && git submodule update --recursive

