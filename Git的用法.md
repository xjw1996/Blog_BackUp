---

title: git的使用方法
date: 2021-5-15 12:16:12
categories: git
tags:
- github
---

# git 简单使用方法

- 从自己电脑上传代码到github。

- ![git](https://img-blog.csdnimg.cn/2021051501291033.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)

- 代码的追加以及更新,如果出现以下错误：

  ```````
  git push origin master
  error: src refspec master does not match any
  error: failed to push some refs to 'git@github.com:user-name/repository-name.git'
  ```````

  <font color="#dd0000">**error: src refspec master does not match any**</font><br />

- 你需要做的是，检查是否<font color="#dd0000">**commit**</font>。<br />

```bash
git add 文件名
git commit -m '提交版本信息'
```


- 如果进行了以上操作，还是出现了<font color="#dd0000">**error**</font>，<br /这是因为GIthub的master branch 变成了 main branch。

  如果你输入以下代码就会解决以上问题。

```bash
#解決
git checkout main
git pull origin main #把github的代码pull到本地
git push origin main #上传自己的新代码文件
```





<font color="#dd0000">**github仓库代码pull前的状态**</font><br />![](https://img-blog.csdnimg.cn/20210515023109518.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)

<font color="#dd0000">**把新的代码上传push到github仓库的状态**</font><br />

![](https://img-blog.csdnimg.cn/20210515023124717.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)

可以看到，从local电脑里更新到github上的文件。







- 具体电脑上运行的流程，以及出错的代码如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210515012937414.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI0MzQ4Ng==,size_16,color_FFFFFF,t_70)