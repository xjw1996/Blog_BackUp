---
title: MySQL编码问题的坑
date: 2020-12-08 12:16:12
categories: MySQL
tags:
- 后端
---



# MySQL编码问题的坑

- 缘由通过前端页码往后端post请求数据更改，中文出现“？？？”的问题![](https://img-blog.csdnimg.cn/20210123192905122.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)
- 进入docker MySQL的交互容器内

```bash
$ sudo docker exec -it mysql /bin/bash
$ mysql -u root -p

# 查看编码方式
$ show variables like 'character\_set\_%';
```
 ![](https://img-blog.csdnimg.cn/20210123194719226.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)
![service mysqld restart](https://img-blog.csdnimg.cn/2021012401044694.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)

储存的中文数据什么也显示不出来，大部分都为latin1，猜想可能这块有问题。

```bash
$ SET character_set_client = utf8;
$ SET character_set_connection = utf8;
$ SET character_set_database = utf8;
$ SET character_set_results = utf8;
$ SET character_set_server = utf8;
```
然后重启MySQL

```bash
$ service mysql restart
```

然后重新查看MySQL的编码形式，然而并没有卵用，MySQL依旧是打不死的小强，无论你你怎么能他依然latin1屹立不倒

- **所以要找到MySQL的my.cof配置文件的位置，对配置文件进行改动**
如下面代码所示，我认为这个是中文不出现乱码的最好的配置

```bash
[client]

default-character-set=utf8

[mysql]

default-character-set=utf8

[mysqld]

init_connect=’SET collation_connection = utf8_unicode_ci’

init_connect=’SET NAMES utf8’

character-set-server=utf8

collation-server=utf8_unicode_ci

skip-character-set-client-handshake

skip-name-resolve
```
所以我又重启了MySQL，看了一遍编码方式我的天latin1纹丝不动我都要吐血了。

查阅了很多资料都是废话连篇，千篇一律。

所以思考是不是配置文件放错地方，导致MySQL根本没有察觉到配置文件的存在，在这里我要说一下我的配置文件原先设置的位置是 etc/mysql/my.conf
一般配置文件是直接放在etc下面的，所以etc下vim了个my.conf，把utf8的配置粘贴了进去，中心启动MySQL服务器，
![](https://img-blog.csdnimg.cn/20210124024319195.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)
清一色utf8，嘎嘎舒服
所以说MySQL的my.cnf这个配置文件放的地方
my.cnf配置文件位置<font color="#dd0000">**超级重要**</font><br />
my.cnf配置文件位置<font color="#dd0000">**超级重要**</font><br />
my.cnf配置文件位置<font color="#dd0000">**超级重要**</font><br />
重要的事说三遍

![](https://img-blog.csdnimg.cn/20210124024434599.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)
前端页码发送post的请求，刷新返回来的数据也显示中文了哈哈。。。。




## 一点应该知道的基础知识
- 小插曲 如果想查看MySQL的配置文件要到/etc/mysql/my.cnf的地方 cat一下可以看到配置信息，**MySQL配置文件在Windows下叫my.ini，在MySQL的安装根目录下；在Linux下叫my.cnf，该文件位于/etc/my.cnf。**
- 我们在创建基础容器之后，进入容器，进行编辑配置文件的时候，需要使用vim或者vi命令，但是会出现：这是因为vim没有安装。

安装vim就好啦
```bash
$ apt-get update
$ apt-get install vim
```
## MySQL的基本操作方法

```bash
$ mysql -u root -p
$ SHOW DATABASES;
$ USE 数据库名称
$ show tables;
$ select * from 表名;
```