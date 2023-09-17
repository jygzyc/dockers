FROM ubuntu:20.04

ENV FRIDA_VERSION 16.1.4
ENV ANDROID_NDK_VERSION r25c
ENV NVM_VERSION v0.39.5


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
RUN set -e && curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/${NVM_VERSION}/install.sh | bash
ENV NVM_DIR $HOME/.nvm
RUN echo 'export NVM_DIR="$HOME/.nvm"'                                       >> "$HOME/.bashrc"
RUN echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm' >> "$HOME/.bashrc"
RUN echo '[ -s "$NVM_DIR/bash_completion" ] && . "$NVM_DIR/bash_completion" # This loads nvm bash_completion' >> "$HOME/.bashrc"
RUN bash -c 'source $HOME/.nvm/nvm.sh' \
    && nvm install --lts \
    && nvm use --lts


# ------------------------------------------------------
# --- Android NDK
ENV ANDROID_NDK_HOME /opt/android-ndk
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


# ------------------------------------------------------
# --- Frida Env
COPY patch /frida-patch
COPY scripts /frida-scripts

ENV FRIDA_ENV_DIR /frida-env
WORKDIR ${FRIDA_ENV_DIR}
VOLUME ${FRIDA_ENV_DIR}

RUN git checkout ${FRIDA_VERSION} \
    && git submodule update --recursive

