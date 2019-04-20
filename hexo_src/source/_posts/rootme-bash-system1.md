---
title: rootme-bash-system1
typora-root-url: rootme-bash-system1
typora-copy-images-to: rootme-bash-system1
date: 2019-03-31 20:32:45
tags:
- rootme
- rootme-pdf-read
---

# Dangers of SUID Shell Scripts



##SUID

SUID：是linux上，文件的一个权限位，就好像rwx。

passwd：

![passwd命令的权限](/1554035810466.png)

![passwd文件的命令](/1554035966147.png)

此文件属于root:root，普通用户有执行的权力，因为有s位，所以使得普通用户在执行passwd的时候提权为root，使得可以拥有w权限对/etc/passwd进行修改自己的密码



[Linux文件特殊权限：SUID、SGID和SBIT](https://blog.csdn.net/sinat_30071459/article/details/51206581)

---



![1554098421524](/1554098421524.png)

