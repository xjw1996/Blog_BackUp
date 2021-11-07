---
title: Docker里面导入MySQL
date: 2019-12-18 12:16:12
categories: docker
tags:
- docker

---

# docker hub

- 下载MySQL镜像

  ```sudo docker pull mysql:5.7```

- 检查下载好的镜像

  ```sudo docker images```

- 切换root的用户

  ```su root```

- 启动容器

  docker run -p 3306:3306 --name mysql \

> -v /mydata/mysql/log:/var/log/mysql \
> -v /mydata/mysql/data:/var/lib/mysql \
> -v /mydata/mysql/conf:/etc/mysql \
> -e MYSQL_ROOT_PASSWORD=root \
> -d mysql:5.7



    -p 3306:3306为端口映射

-v 是目录挂载 具体意思是在本体linux下的/mydata/mysql/log  ：冒号就是与容器内部的/var/log/mysql进行挂载，直白的说容器内部log文件的记录 会直接反应到linux指定目录下，容器内部的日志，linux在外面也能够看到

![](https://img-blog.csdnimg.cn/20210120010836980.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)


如上图所示，MySQL容器挂载的各个log，数据，配置文件dg在外面root linux里面也是可以访问到的，容器外部对挂载文件直接修改，是可以同步到容器



- 查看docker运行中的容器

  ```docker ps```

- 进入MySQL容器内部

  - ```docker exec -it docker容器的前三位id\或者容器的名字 /bin/bash```

 ![](https://img-blog.csdnimg.cn/2021012001085931.png)

  - 就会发现我们进入容器内部的环境当中，因为每一个容器都是一个小型的linux系统，如上图为哦们已经进入到MySQL容器的内部

  ![](https://img-blog.csdnimg.cn/20210120010917607.png)


  - 查看目录结构会发现，他就是一个完整的一个linux的目录结构

![](https://img-blog.csdnimg.cn/20210120011025911.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)


- 查看MySQL装在了那个位置

  ```whereis mysql```
![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-uuUv48u2-1611076069706)(C:\Users\jinue\AppData\Roaming\Typora\typora-user-images\image-20201206041528226.png)\]](https://img-blog.csdnimg.cn/20210120011051134.png)

- 下图是docker容器文件挂载与端口映射
![](https://img-blog.csdnimg.cn/20210120011126996.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)

访问linux的3306接口也就能够访问到MySQL容器的3306的端口

cd conf      vi my.cnf

- 复制下面配置文件

[client]

default-character-set=utf8

[mysql]

default-character-set=utf8



[mysqld]

init_connect='SET collation_connection = utf8_unicode_ci'

init_connect='SET NAMES utf8'

character-set-server=utf8

collation-server=utf8_unicode_ci

skip-character-set-client-handshake

skip-name-resolve

![](https://img-blog.csdnimg.cn/20210120011245615.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)

- docker 重启mysql

  ```docker restart mysql```

  

- 进入交互模式

  ```docker exec -it mysql /bin/bash```

  查看linux上配置的文件是否同步到位

  - 进入MySQL内部查看配置
  - ```mysql -u root -p```





