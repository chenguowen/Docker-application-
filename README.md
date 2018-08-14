https://blog.csdn.net/qq_36892341/article/details/73918672
https://blog.csdn.net/bingzhongdehuoyan/article/details/79411479 
https://blog.csdn.net/fei2636/article/details/79384969 
https://blog.csdn.net/s_kay/article/details/78416703


https://www.nvidia.cn/object/docker-container-cn.html 
清华的tuna真是神器： 如何安装上docker-ce  
https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/  

安装nvidia-docker的办法 ： 
https://github.com/NVIDIA/nvidia-docker 

毛病不大也。基本是对的 关键的看tuna
https://blog.csdn.net/menghuanbeike/article/details/79212195

sudo service docker  start    开启docker服务 
sudo service docker status   查看 docker状态 
docker load c222
nvidia-docker run  --shm-size=8gb   -it -v `pwd`:/workspace c22  
sudo docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi  下载的nvidia

最初load一个外面的文件，放在当前目录里面用./*****
sudo docker load   -i  ./guowen  
再用sudo docker images 就可以看到一个image了，应该ID是c22****。就成功导入
装好nvidia-docker就能愉快的用了

设置虚拟内存8gb，当前路径为workspace，采用ID为c22<something else>的image
nvidia-docker run  --shm-size=8gb   -it -v `pwd`:/workspace c22  

保存docker
sudo docker save -o guowen  6afbf 



基本是对的 关键的看tuna  ； https://blog.csdn.net/menghuanbeike/article/details/79212195

安装nvidia-docker的办法 ： https://github.com/NVIDIA/nvidia-docker 


清华的tuna真是神器： 如何安装上docker-ce  https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/  
6:36

最初load一个外面的文件，放在当前目录里面用./*****sudo docker load   -i  ./guowen

再用sudo docker images 就可以看到一个image了，应该ID是c22****。就成功导入装好nvidia-docker就能愉快的用了

设置虚拟内存8gb，当前路径为workspace，采用ID为c22<something else>的imagenvidia-docker run  --shm-size=8gb   -it -v `pwd`:/workspace c22    


linux版本：Ubuntu16.04
第一次安装Docker，运行docker命令是可以的，如
[html] view plain copy
  1. docker ps  
重启系统之后，运行docker ps后出现如下报错：
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
查找资料后，知道了这是权限问题。需要root用户才能运行。

从0.5.2开始docker的守护进程总是以root用户来运行。docker守护进程绑定的是Unix的socket而不是一个TCP端口。Unix的socket默认属于root用户，所以，使用docker时必须加上sudo。
从0.5.3开始，创建一个名为docker组，然后将用户加入这个组内。当docker守护进程启动时，它会把Unix的读写权限赋予docker组。这样，当你作为docker组内用户使用docker客户端时，你就无须使用sudo了。

----分割线---
以下为解决办法：
第一种：
依次运行以下命令，跳转至root用户去运行docker命令：
[html] view plain copy
  1. sudo su                       //切换到root  
  2. service docker start      //启动docker service  
  3. docker images              //显示所有images  
  4. docker ps //重新运行docker命令  
第二种：
把当前用户加到docker用户组中：
# 添加docker用户组
[html] view plain copy
  1. sudo groupadd docker  

# 把自己加到docker用户组中
[html] view plain copy
  1. sudo gpasswd -a myusername docker  

# 重启docker后台服务
[html] view plain copy
  1. sudo service docker restart  

重启系统，直接运行docker命令就行了，不用加上sudo。




*********************************************************





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
