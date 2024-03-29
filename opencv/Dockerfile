ARG BASE="ubuntu:18.04"
ARG VERSION="4.4.0"
ARG CUDA_ARCH_BIN="5.0,5.2,6.0,6.1,7.0,7.5,8.6"

FROM $BASE
ARG VERSION
ARG CUDA_ARCH_BIN

# install build tools and dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        build-essential \
        git \
        cmake \
        pkg-config \
        libgtk-3-dev \
        libtbb2 \
        libtbb-dev \
        libeigen3-dev \
        libopenblas-dev \
        liblapacke-dev \
        libjpeg-dev \
        libpng-dev \
        libtiff-dev \
        libtiff5-dev \
        libavcodec-dev \
        libavformat-dev \
        libswscale-dev \
        libavutil-dev \
        libxvidcore-dev \
        libx264-dev \
        libv4l-dev \
        python-pip \
        python-dev \
        python-numpy \
        python3-pip \
        python3-dev \
        python3-numpy \
        gstreamer1.0-doc \
        gstreamer1.0-tools \
        gstreamer1.0-libav \
        gstreamer1.0-plugins-base \
        gstreamer1.0-plugins-good \
        gstreamer1.0-plugins-bad \
        gstreamer1.0-plugins-ugly \
        libgstreamer1.0 \
        libgstreamer1.0-dev \
        libgstreamer-plugins-base1.0-dev \
        libgstrtspserver-1.0 \
        libgstrtspserver-1.0-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* 

# opencv from source code 
WORKDIR /opt
RUN git clone -b ${VERSION} --depth 1 https://github.com/opencv/opencv.git && \
    git clone -b ${VERSION} --depth 1 https://github.com/opencv/opencv_contrib.git && \
    cd opencv && \
    mkdir build && cd build && \
    cmake -D CMAKE_BUILD_TYPE=RELEASE \
        -D CMAKE_INSTALL_PREFIX=/usr/local \
        -D OPENCV_GENERATE_PKGCONFIG=YES \
        -D OPENCV_GENERATE_SETUPVARS=NO \
        -D OPENCV_ENABLE_NONFREE:BOOL=ON \
        -D BUILD_opencv_python2=ON \
        -D BUILD_opencv_python3=ON \
        -D PYTHON_EXECUTABLE=$(which python2) \
        -D PYTHON3_EXECUTABLE=$(which python3) \
        -D PYTHON_DEFAULT_EXECUTABLE=$(which python2) \
        -D PYTHON2_INCLUDE_DIR=$(python2 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
        -D PYTHON2_PACKAGES_PATH=$(python2 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \
        -D PYTHON3_INCLUDE_DIR=$(python3 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
        -D PYTHON3_PACKAGES_PATH=$(python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \
        -D WITH_CUDA=ON \
        -D WITH_CUBLAS=ON \
        -D CUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda \
        -D CUDA_ARCH_BIN=${CUDA_ARCH_BIN} \
        -D CUDA_ARCH_PTX="" \
        -D WITH_GSTREAMER=ON \
        -D WITH_GSTREAMER_0_10=OFF \
        -D WITH_FFMPEG=ON \
        -D WITH_V4L=ON \
        -D WITH_1394=OFF \
        -D ENABLE_PRECOMPILED_HEADERS=OFF \
        -D WITH_OPENGL=OFF \
        -D WITH_OPENCL=OFF \
        -D WITH_QT=OFF \
        -D BUILD_PROTOBUF=OFF \
        -D BUILD_TESTS=OFF \
        -D BUILD_PERF_TESTS=OFF \
        -D BUILD_EXAMPLES=OFF \
        -D INSTALL_TESTS=OFF \
        -D INSTALL_PYTHON_EXAMPLES=OFF \
        -D INSTALL_C_EXAMPLES=OFF \
        -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules \
        .. && \ 
    make -j"$(nproc)" && make install && ldconfig && \
    cd /opt && rm -rf opencv && rm -rf opencv_contrib 

CMD ["bash"]
