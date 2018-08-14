https://blog.csdn.net/qq_36892341/article/details/73918672
https://blog.csdn.net/bingzhongdehuoyan/article/details/79411479 
https://blog.csdn.net/fei2636/article/details/79384969 
https://blog.csdn.net/s_kay/article/details/78416703

# Docker-application-
Docker 镜像建立 与 标记 导出 使用 等等

一些开源代码会给出类似与Makefile的Dockerfile，
将其放到类似"ubuntu"的文件夹中并在运行
testtest@hhh:~/ceres-solver-1.14.0/build$   sudo docker build "ubuntu"  
即可生成对应的docker，

Dockerfile愈发怪异，类似于Makefile：

FROM nvidia/cuda:8.0-devel
ARG CUDA_GENERATION=Auto

RUN apt-get update && apt-get install -y \
    clang \
    cmake \
    git \
    libblas-dev \
    libeigen3-dev \
    libgoogle-glog-dev \
    libgtk2.0-dev \
    liblapack-dev \
    libproj-dev \
    libsuitesparse-dev \
    libvtk5-dev \
    pkg-config \
    zip
# Build dynfu
WORKDIR dynfu/build
RUN cmake -D CUDA_CUDA_LIBRARY="/usr/local/cuda/lib64/stubs/libcuda.so" ..
RUN make
WORKDIR ..

# Run dynamicfusion using /data
CMD ./build/bin/app /data

# Rmeove unnecessary packages
RUN apt-get remove -y \
    clang \
    curl \
    git \
    pkg-config \
    zip
