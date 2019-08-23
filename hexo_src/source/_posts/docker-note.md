---
title: '''docker-note'''
typora-root-url: '''docker-note'''
typora-copy-images-to: '''docker-note'''
date: 2019-08-08 20:31:46
tags: docker
---

Learn From [Bilibili video - 尚硅谷Docker核心技术](https://www.bilibili.com/video/av49095718/?p=17)

#### docker run

用一个镜像产生一个容器

-d 后台运行这个容器, 不会得到交互窗口

-it 得到容器的交互式窗口 shell.

-p 端口映射 

> -p 主机端口:docker容器端口

-P 随机分配端口

> -P 不跟端口号, 但是他是怎么知道要映射到docker容器的哪一个端口?

-i 交互

-t 终端

-v 挂载 宿主机的数据目录

> docker run -it -v /myDataVolume:/dataVolInContainer 镜像名
>
> docker run it -v /myVol:/data:ro 镜像名  只读卷, 主机能写, 容器只读.

#### docker inspect

获取容器的json格式的信息

#### docker ps

输出当前正在运行的容器

`docker ps -a` 输出所有的容器

`docker ps -q` 安静输出, 只输出镜像的id值, 可以作为其他命令的输入:

> docker rm -f $(docker ps -q)  -- 删除当前所有在运行的容器

#### docker rm

好像是用来删除容器的

#### docker commit

提交容器副本成为镜像

> docker commit -m="提交信息" -a="作者" 容器ID 要创建的镜像的名字:标签号
>
> docker commit -m="tomcat without doc" -a="dog"  f23k02j dogdog/tomcat:1.21



## 容器数据卷

-v