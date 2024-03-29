FROM ubuntu:18.04

MAINTAINER m4j0rt0m

# install prerequisite software
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
    build-essential clang bison flex libreadline-dev \
    gawk tcl-dev libffi-dev git mercurial graphviz \
    xdot pkg-config python python3 libftdi-dev \
    qt5-default python3-dev cmake libeigen3-dev libboost-all-dev && \
    apt-get clean

ENV DOCK /opt/docker
RUN mkdir $DOCK
WORKDIR $DOCK

# install icestorm tools
RUN cd $DOCK && \
    git clone https://github.com/YosysHQ/icestorm.git icestorm && \
    cd icestorm && \
    make -j$(nproc) && \
    make install

# install arachne-pnr
RUN cd $DOCK && \
    git clone https://github.com/cseed/arachne-pnr.git arachne-pnr && \
    cd arachne-pnr && \
    make -j$(nproc) && \
    make install

# install cmake
RUN apt-get install -y libssl-dev wget && \
    cd $DOCK && \
    wget https://github.com/Kitware/CMake/releases/download/v3.16.5/cmake-3.16.5.tar.gz && \
    tar -zxvf cmake-3.16.5.tar.gz && \
    cd cmake-3.16.5 && \
    ./bootstrap && make && \
    make install

# install nextpnr
RUN cd $DOCK && \
    git clone https://github.com/YosysHQ/nextpnr nextpnr && \
    cd nextpnr && \
    git submodule update --init --recursive && \
    cmake --version && \
    cmake -DARCH=ice40 && \
    make -j$(nproc) && \
    make install

# install yosys
RUN cd $DOCK && \
    git clone https://github.com/YosysHQ/yosys.git yosys && \
    cd yosys && \
    make -j$(nproc) && \
    make install

# remove downloaded stuff
RUN cd $DOCK && \
    rm -rf ./*

# install some extra stuff
RUN apt-get install -y rsync

# copy rules
COPY 54-ft2232.rules /etc/udev/rules.d/54-ft2232.rules

# force the use of bash
RUN ln -sf /bin/bash /bin/sh

WORKDIR /root
