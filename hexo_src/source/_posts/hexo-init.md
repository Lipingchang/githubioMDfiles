---
title: hexo init
date: 2019-03-31 14:06:24
typora-root-url: hexo-init
typora-copy-images-to: hexo-init
tags:
- hexo
---

# Hexo init

---

>**NOTE**：
>
>hexo new "文章标题"
>
>hexo g
>
>git push 上传md文件
>
>git subtree push -P dist sub_origin master  上传网页

---



### 安装

用npm安装hexo-cli

`npm install hexo-cli -g`

然后找个目录初始化项目结构：

`hexo init`

最后

`hexo g` 用来把md文件转换成html文件

> 可在配置文件_config.yml中找到：public_dir 这个字段，然后可以设置生成的html文件的路径：
>
> ![1554030359304](/1554030359304.png)



### GitHub 配置

#### 随便配置

- 要先有个项目，项目名字必须是 `用户名.github.io`
- 在项目的settings里面 GitHub Pages 中配置
- 最后把hexo g生成的文件push到这个项目中

#### 使用 git subtree

username.github.io 要求该仓库的根目录必须是网站的更目录。为了方便管理，就要把hexo的src和dist分成两个仓库。

* 建两个仓库：A - hexo的主目录；B - hexo的dist，就是叫做username.github.io的那个。

* 在本地仓库A中，添加一个B的远程别名：

  `git remote add sub_origin https://github.com/username/username.github.io`

* 如果**远程仓库是空的**话，就可以直接`hexo g`后把dist中的内容push到sub_origin中：

  `git subtree push --prefix dist(生成目录的名字) sub_origin master `

  **不是空的**：

  `git subtree pull --prefix dist sub_origin master`然后再`hexo g`覆盖掉之前的内容，再push。

因为 B 是 A 中的子项目，所以要在A中把dist文件夹中的内容都commit后，才能让子项目识别到这内容。

#### 更加简单

配置hexo配置文件中的deploy属性。

。。。。。。。

> [添加现有的repo为Git Subtree](https://hisoka0917.github.io/git/2018/04/23/add-exist-repo-as-subtree/)

### 图片上传

#### 平常方法

要先装个 `npm install hexo-asset-image --save`  使得hexo可以识别出md中有关图片的语句。

下面几种方法可以让hexo引用你放在文件夹中的图片：

> - **所有的图片在同一个文件夹下**    （方法一）
>
> 在source目录下建立文件夹（名字自己起）就是专门拿来放图片的；
>
> 使用的时候用 `![](/image_folder_name/image.jpg)`就可以引用了。
>
> - **图片按照文章（post）组织**    （方法二）
>
> 把 _config.yml 中的 post_asset_folder 设置为 true：![1554020008257](/1554020008257.png)
>
> `这会让hexo new "title"`的时候在_posts文件夹中同时创建md文件和同名的文件夹：![1554020094936](/1554020094936.png)
>
> 那么在使用图片的时候就直接用 `![](image.jpg)` 来连接这个图片
>
> - **用网络图床啥的**
>
> 不提
>
> - **其他语法**
>
> // 语法
>
> &#123;%img [class names] /path/to/image [width] [height] [title text [alt text]] %&#125;
>
> // 实例
>
> &#123;% img full-image /hexo-experiences/PL01.jpg 180 180 hello %\&#125;
>
> // 生成的代码
>
> `<img src="/blog/hexo-experiences/PL01.jpg" class="full-image" width="180" height="180" title="hello">`
>
> 
>
> 
>
> [Hexo 插入图片的几种方法](<http://www.mashangxue123.com/Hexo/1322489026.html#2-hexo-fang-shi-post-asset-folder>)
>
> [Hexo博客搭建之在文章中插入图片](https://yanyinhong.github.io/2017/05/02/How-to-insert-image-in-hexo-post/)



#### 编辑器中的方法

我用的是 Typora 编辑器，截图用的是 Evernote 的快捷键，用微信的截图也可以，都可以从剪贴板Ctrl+v 导入到编辑器中，所以要配置其他的选项使得：Typora 导入图片的时候会把图片到入到上面几个方法的文件夹中。

- 首先配置编辑器中的“偏好设置”，”优先使用相对路径“：

  ![1554021044613](/1554021044613.png)

- 然后在文章头部的 `YAML Front Matter` 中配置这篇文章的图片复制的路径：

  typora-root-url: xxx

  typora-copy-image-to: xxxx

  ![1554021244098](/1554021244098.png)

  就会让所有复制进 这篇文章的图片 都放到该目录下面（上图中的配置是给方法二用的）

  方法一的可以写下面这个（没试过）：

  ![1554021512389](/1554021512389.png)

最后是把 typora-root-url 和 typora-copy-image-to 两个属性放到模板文件中：

hexo_root\scaffolds\post.md：

![1554030019028](/1554030019028.png)

这个模板会在每次hexo new的时候创建



> [使用 Typora 在 Hexo 中写博客完美处理本地图片](https://www.anynote.me/1036055241.html)

