---
title: Win10Docker Desktop(WSL2) pull的image等从C盘移植其他盘
date: 2021-01-30 21:36:37
categories: docker
tags:
- docker

---

# Win10Docker Desktop(WSL2) pull的image等从C盘移动到其它盘
缘由：由于c系统盘储存空间有限，Docker的桌面版默认安装在C盘，长此以往电脑储存空间就会不够用，把c盘Docker的文件移动到其他盘是唯一的选择。

```bash
$ wsl -l -v --all
```
![](https://img-blog.csdnimg.cn/20210130201404860.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)

- docker-desktop是存放程序的
- docker-desktop-data是存放镜像的

## (1)Clean/Purge data  删除镜像 wsl hyperv
![](https://img-blog.csdnimg.cn/20210130201625903.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)
##  (2)打包wsl系统镜像
```bash
$ wsl --export docker-desktop docker-desktop.tar
$ wsl --export docker-desktop-data docker-desktop-data.tar
```
![](https://img-blog.csdnimg.cn/2021013020181257.png)
##  (3)解除现有登录的wsl子系统

```bash
$ wsl --unregister docker-desktop
$ wsl --unregister docker-desktop-data
```
##  (4)把刚刚打包过的系统文件，导入到自己想要的盘下面

```bash
$ wsl --import docker-desktop d:\your-install-path docker-desktop.tar
$ wsl --import docker-desktop-data d:\your-install-path docker-desktop-data.tar
```
![](https://img-blog.csdnimg.cn/2021013020214070.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)
- **这里需要注意：**，请不要把docker-desktop和docker-desktop-data同时放到同一个文件夹里面，导入到两个名字不同的文件夹里就OK了，然后重启**docker desktop**。如上图所示。
- ![](https://img-blog.csdnimg.cn/20210130202401881.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)
## Pull一个镜像试一试，看看C盘的存储是否增加
Pull一个ROS的镜像，大小有3个G左右。
![](https://img-blog.csdnimg.cn/20210130202555124.png)
我们先看一下，**F盘**下的存储docker image的地方，存储是否增加。
![](https://img-blog.csdnimg.cn/20210130202924280.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20210130202936112.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20210130202945608.png)

- 由上图可以见到，F盘随着pull的时间，储存量越来越多，C盘的存储却不会变，**大吉大利，今晚吃鸡！！！！** 嘎嘎嘎