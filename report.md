打开特定的docker ： 
当前路径为workspace   
nvidia-docker run -it -v `pwd`:/workspace f9d8  
exit退出 


保存docker
sudo docker save -o guowen  6afbf 


docker save 可以保存特定文件
nvidia-docker run -it -v `pwd`:/workspace c22edcc

保存docker
docker commit 7106e0 

标记docker   
docker tag c22ed    guowen 

查看目前的docker: 
docker images   


*********************************************************

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

