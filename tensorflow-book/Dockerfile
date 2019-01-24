ARG BASE_CONTAINER=huanghe314/tensorflow-book-gpu:0.0.1
FROM $BASE_CONTAINER

LABEL maintainer="zhenyu liu"

USER root

RUN apt-get update \
    && apt-get install -y \
        build-essential \
        cmake \
        git \
        wget \
        unzip \
        yasm \
        pkg-config \
        libswscale-dev \
        libtbb2 \
        libtbb-dev \
        libjpeg-dev \
        libpng-dev \
        libtiff-dev \
        libavformat-dev \
        libpq-dev \
        libavcodec-dev \
        libavformat-dev \
        libswscale-dev \
        libv4l-dev \
        libxvidcore-dev \
        libx264-dev \
        libgtk-3-dev \
        libatlas-base-dev \
        gfortran \
        python3-dev \
    && rm -rf /var/lib/apt/lists/*

RUN conda install numpy

WORKDIR /
ENV OPENCV_VERSION="4.0.0"
RUN cd / \
&& wget https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip \
&& unzip ${OPENCV_VERSION}.zip \
&& rm /${OPENCV_VERSION}.zip

RUN cd / \
&& wget https://github.com/opencv/opencv_contrib/archive/${OPENCV_VERSION}.zip \
&& unzip ${OPENCV_VERSION}.zip \
&& rm /${OPENCV_VERSION}.zip

RUN apt-get update \
    && apt-get autoremove -y libtiff5-dev \
    && apt-get install -y \
       libtiff5-dev \
    && rm -rf /var/lib/apt/lists/*

RUN cd opencv-${OPENCV_VERSION}/ \
&& mkdir build \
&& cd build \
&& cmake -D CMAKE_BUILD_TYPE=RELEASE \
  -D CMAKE_INSTALL_PREFIX=/usr/local \
  -D INSTALL_PYTHON_EXAMPLES=ON \
  -D INSTALL_C_EXAMPLES=OFF \
  -D OPENCV_ENABLE_NONFREE=ON \
  -D OPENCV_EXTRA_MODULES_PATH= /opencv_contrib-${OPENCV_VERSION}/modules \
  -D PYTHON_EXECUTABLE=/opt/conda/bin/python \
  -D BUILD_PROTOBUF=OFF \
  -D BUILD_opencv_dnn=OFF \
  -D WITH_CUDA=ON \
  -D ENABLE_FAST_MATH=1 \
  -D CUDA_FAST_MATH=1 \
  -D WITH_CUBLAS=1 \
  -D BUILD_EXAMPLES=ON .. \
&& make \
&& make install \
&& ldconfig

RUN ln -s \
  /opt/conda/python/cv2/python-3.6/cv2.cpython-36m-x86_64-linux-gnu.so \
  /opt/conda/lib/python3.6/site-packages/cv2.so
