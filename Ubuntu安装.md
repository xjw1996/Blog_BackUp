---
title: Ubuntu安装 NVIDIA Driver，CUDA, cuDNN
date: 2020-04-20 12:16:12
categories: Ubuntu
tags:
- Linux
---

- 用USB引导盘装好系统以后，选英语语言和英语键盘并且加入字体库命令

```bash
$ ls /usr/lib/locale
$ sudo locale-gen - -purge - -no-archive
```

 1. 下载 NVIDIA  Driver

| CUDA Toolkit               | Linux x86_64 Driver Version |
| -------------------------- | --------------------------- |
| CUDA 9.2(9.2.88)           | >=396.26                    |
| CUDA 9.2(9.2.148 Update 1) | >=396.37 && <410.48         |
| CUDA 10.1                  | >=418.39                    |
| CUDA 10.2.89               | >=440.33                    |
| CUDA 11.0.1 RC             | >=450.36.06                 |
| CUDA 11.0.2 GA             | >=450.51.05                 |
| CUDA 11.0.3 Update 1       | >=450.51.06                 |

2. NVIDIA  Driver 安装

在google搜索nvidia drivers找到官网，下载驱动

```bash
$ sudo apt-get purge nvidia*
$ sudo apt-get autoremove
$ sudo apt-get install build-essential gcc-multilib dkms
$ sudo touch /etc/modprobe.d/blacklist-nouveau.conf
$ sudo gedit /etc/modprobe.d/blacklist-nouveau.conf
```
输入
blacklist nouveau
options nouveau modeset=0
进入/etc/default 找到grub文件，在此⻚打开terminal，输入命令

```bash
$ sudo gedit grub
```
在 GRUB-CMDLINE-LINUX-DEFAULT=“quiet splash“处的splash 后加nomodeset
```bash
$ sudo update-initramfs -u 
$ sudo update-grub2
$ cd ~
$ cd /Downloads
$ ls -la
$ sudo chmod +x NVIDIA-Linux-x86_64-450.57.run 
$ sudo ./NVIDIA-Linux-x86_64-450.57.run
```
3. 安装CUDA

去官网找到相应版本，installer Type选deb(local)

```bash
$ sudo nano /etc/profile.d/cuda.sh
```
输入

```bash
export PATH=$PATH:/usr/local/cuda/bin
export CUDADIR=/usr/local/cuda
```

```bash
$ sudo chmod +x /etc/profile.d/cuda.sh 
$ sudo nano /etc/ld.so.conf.d/cuda.conf
```
输入

```bash
/usr/local/cuda/lib64
```
```bash
$ sudo ldconfig 
$ reboot
$ nvcc - -version
```
4. cuDNN 的安装
官方网站下载cudnn
- cuDNN Library for Linux (x86)
- cuDNN Runtime Library for Ubuntu 18.04
- cuDNN Developer Library for Ubuntu 18.04
- cuDNN Code Samples and User Guide for Ubuntu 18.04
- 如果双击后在软件仓库安装时出现无法俺从来而情况，可以通过命令安装

```bash
sudo dpkg -i 要解压的文件
```
4.1 将文件解压并复制到CUDA中

```bash
$ tar -xzvf cudnn-ooxx.tgz //解压文件，也可手动解压
$ sudo cp cuda/include/cudnn.h /usr/local/cuda/include
$ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```
4.2 检验cuDNN是否安装成功
```bash
$ cp -r /usr/src/cudnn_samples_v7/mnistCUDNN $HOME
$ cd $HOME/cudnn_samples_v7/mnistCUDNN 
$ make clean && make
$ ./mnistCUDNN
```
出现了Test passed！ 则安装成功

如果命令copy过去没有编译成功 可以手动复制过去

![](https://img-blog.csdnimg.cn/20210120045448638.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/20210120045501656.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)