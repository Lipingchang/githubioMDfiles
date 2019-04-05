---
title: flutter安装
typora-root-url: flutter安装
typora-copy-images-to: flutter安装
date: 2019-04-05 10:12:40
tags:
- flutter
---

# 有个好用的代理

因为GFW，Flutter的安装寸步难行，用SSTap代理所有程序的流量，就能解决所有问题，就可以跟着官方教程顺畅的走下来。

# 去他的官网下载

<https://flutter.dev/docs/get-started/install/windows>

![1554430632539](/1554430632539.png)

解压得到：

![1554430686503](/1554430686503.png)

运行flutter_console.bat，进入cmd，就可以用`flutter`命令来创建应用了。

先用`flutter doctor`检查开发环境：

![1554430821718](/1554430821718.png)

中间如果报错的话，可以用 flutter doctor -v 输出详细过程。

#### 我的两个报错：

- 第一次配的时候，他说缺少ANDROID_HOME，也就是Android的sdk的路径，就在用户的环境变量中配置了：

![1554430977094](/1554430977094.png)

并加入到用户的Path变量中

- 然后是有个android licenses 啥的错误，要先运行他给提示你的命令，然后接受里面的证书，就ok了，但是由于GFW，有时候就加载的很慢。（其实他调用的是sdk的tools中的 sdkmanager.bat --licenses命令。）![1554431264016](/1554431264016.png)



# 配置你的编辑器

我用的是Android Studio，用SSR的可以在AS的配置中找proxy，然后配置。

![1554431339613](/1554431339613.png)

去他的设置的plugins中下载flutter和dart的插件，重启ide，用`flutter doctor`再检测下，就能通过所有的检查。

# 开始第一个项目

配置好AS后，就可以在他的菜单中找到：

![1554431712765](/1554431712765.png)

然后设置刚刚下载的flutter sdk的路径

![1554431977205](/1554431977205.png)

后面就ok了。

