FROM ubuntu:20.04

# Change apt sources list
COPY sources.list /etc/apt/sources.list

# Create environment
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get -qq update
RUN apt-get -y install bc bison build-essential ccache cpio curl flex g++-multilib gcc-multilib 
RUN apt-get -y install git git-lfs gnupg gperf imagemagick libc6-dev libelf-dev libgl1-mesa-dev liblz4-tool
RUN apt-get -y install libncurses5 libncurses5-dev libsdl1.2-dev libssl-dev libx11-dev libxml2 libxml2-utils 
RUN apt-get -y install lzop lzip m4 make ncurses-dev patch pngcrush python3 python3-pip rsync schedtool 
RUN apt-get -y install squashfs-tools unzip x11proto-core-dev xsltproc zip zlib1g-dev openjdk-11-jdk
RUN ln -s /usr/bin/python3 /usr/bin/python

# Install repo
RUN curl https://storage.googleapis.com/git-repo-downloads/repo > /usr/bin/repo

# Turn on caching
# ENV USE_CCACHE 1
# ENV CCACHE_EXEC /usr/bin/ccache
# RUN ccache -M 50G

# Mount source code directory
VOLUME /source
ENV WORKDIR /source
WORKDIR $WORKDIR


