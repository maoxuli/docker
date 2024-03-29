ARG BASE="ubuntu:18.04"
ARG VERSION="1.38.1"
ARG ARCH="x86_64"

FROM $BASE AS build
ARG BASE
ARG VERSION
ARG ARCH

## install build tools and dependencies 
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        build-essential \
        wget \
        git \
        pkg-config \
        gengetopt \
        autogen \
        autoconf \
        automake \
        libtool \
    && rm -rf /var/lib/apt/lists/* 

# upgrade cmake 
WORKDIR /opt
RUN wget --show-progress --progress=bar:force -O cmake-linux.sh https://github.com/Kitware/CMake/releases/download/v3.20.5/cmake-3.20.5-linux-${ARCH}.sh && \
    sh cmake-linux.sh -- --skip-license --prefix=/usr && \
    rm cmake-linux.sh

# grpc from source code  
WORKDIR /opt
RUN git clone --recurse-submodules -b v${VERSION} --depth 1 https://github.com/grpc/grpc && \
    cd /opt/grpc/third_party/protobuf && \ 
    ./autogen.sh && ./configure && \
    make && make install && ldconfig && \
    cd /opt/grpc && \
    mkdir -p cmake/build && \
    cd cmake/build && \
    cmake -DgRPC_INSTALL=ON -DgRPC_BUILD_TESTS=OFF ../.. && \
    make -j"$(nproc)" && make install && \
    cd /opt/grpc/third_party/abseil-cpp && \
    mkdir -p cmake/build && \
    cd cmake/build && \
    cmake -DCMAKE_POSITION_INDEPENDENT_CODE=TRUE ../.. && \
    make -j"$(nproc)" && make install 

# FROM $BASE

# grpc runtime  
WORKDIR /opt
RUN rm -rf grpc 

CMD ["/bin/bash"]
