---
title: viewports
typora-root-url: viewports
typora-copy-images-to: viewports
date: 2019-04-22 17:27:38
tags:
---



# 桌面浏览器上的viewport

译：[A tale of two viewports - part one](https://www.quirksmode.org/mobile/viewports.html)

介绍viewpoints是啥，以及一些重要的页面元素（比如 `<html>` ，window，screen）的宽度（width）是怎么确定的**

首先会介绍有关桌面浏览器的相关术语，这会对之后讨论有关手机上的浏览器的相关知识有所帮助。



# 概念 : 设备像素 和 CSS像素

设备像素就是屏幕的像素，通常是1080*1920。

CSS像素是浏览器中的像素，这个值会随着浏览器的缩放变动。

![1556260760807](/1556260760807.png)

放大10%之后，1.1个Device Pixel对应1个CSS像素，于是页面就变大。



### 获取像素的JS接口

screen对象可以获取 **设备像素**

`screen.height` `screen.width`

![1556261093189](/1556261093189.png)

window对象可以获取 **CSS像素**

`window.innerHeight` ` window.innerWidth`

在放大为200%之后，CSS像素就变成Device像素的一半了，outerWidth/Height 倒是和Device像素是一样的，不管怎么缩放浏览器，比例都是不变的。

![1556261393708](/1556261393708.png)

页面滚动的长度单位是CSS像素，所以不管页面怎么缩放，`window.pageOffset` 的值都是固定的。



# 概念：viewport

viewport是用来约束（constrain）你的页面中最顶层的 `<html` 元素的大小的。

- 例子：

  你设计了一个可以自动调整大小的页面（liquid layout），页面中的侧边栏`width: 10%`，使得在浏览器窗口resize的时候，侧边栏的大小会自动的变动。

  

  具体原因是，这个侧边栏长度定制为其parent的10%，现在假定他的parent是`body`，并你没有给`body`标签设置width，那么`body`的长度是谁定的？

  

  通常，所有块级元素都会占据其父亲的100%的width（虽然有例外，但是先不提），所以`body`和`html`的width是相等的。

  最后是`html`元素的大小，他相等于浏览器窗口的width，所以你的侧边栏的width会随着浏览器窗口大小的变动而变动。

  

  但是在理论/实现中（in theory）html和浏览器窗口中间还隔着一个viewport，`<html>`元素的width被viewport的width所限制。使得`<html>`元素占据了viewport的所有width。viewport的大小才是完全的等于浏览器的窗口大小：他就是被这样子定义的。但是viewport并不是HTML中的一个结构体，所以不能被CSS样式所影响。**在桌面环境中，viewport的长宽和浏览器的大小是一样的，但是在手机上就会有点不同/复杂了。**

# viewport的后果

viewport在没有缩放的页面上工作的会挺好的，但是如果你的页面放大之后就会？？

在100%缩放条件下，两个div是这样的：

![1556265134351](/1556265134351.png)

但是放大到200%的时候，固定长度的`1000px box`会比浏览器窗口的长度要长，就会顶开浏览器窗口，产生横向的滚动条；向右边滚动后，会发现`100%box`并没有填充到滚动条之外的区域。

![1556265226573](/1556265226573.png)

![1556265251836](/1556265251836.png)

因为`100%box` 他的 (CSS像素长度) == (viewport的宽度) == (浏览器的宽度) ，使得他填满了viewport的长度，但是没有填满被`1000px box`顶开的长度。

那么只要不要让滚动条出现，就不会让`100%box`露馅，之前我们知道`<html>`元素是和viewport一样大小的，那么只要让溢出html的元素都hidden就不会有滚动条出现了：`html {overflow: hidden;}`。



### 题外话 document width?

因为上面的问题，原作者想引入一种新的JS接口，可以获取这个页面的真正的长度，包括那些撑开页面大小的元素。（他说但是好像没有这种接口，虽然你可以自己去计算）

但是这个会有问题的吧，就好像是生鸡生蛋的问题，一旦有元素用了这个属性，那么就意味着这个元素需要在所有元素（包括这个元素）渲染完成之后才可以渲染，那么，谁先渲染？？



# viewport的测量/获取

可以用 `document.documentElement.clientWidth/Height` 来获取viewport的大小。

在DOM中，`document.docuemntElement` 其实指的是`<html>` 元素，也就是HTML文档中的根元素，即使viewport是包裹了`<html>`的。

即使你给`<html>`设置了width之后，使用 `document.documentElement.clientWidth/Height` 获取到的大小还是viewport的大小，而不是设置后`html`的大小。

![1556343660364](/1556343660364.png)

可以看到offsetWidth是设置的width的大小，而clientWidth不变，还是viewport的大小。

所以`document.documentElement.clientWidth` 和`-Height` 总是给出viewport的尺寸，从而忽略掉 `<html>` 的尺寸。



## 两种获取方式

`window.innerWidth/Height` 和 `document.documentElement.clientWidth/Height` 之间的区别：

是否 包括 **下拉滑块** 的宽度：

![1556345460234](/1556345460234.png)

在缩放为100%的情况下，下拉滑块 的宽为 17 CSS px，闲的无聊的，可以截图数一数（蓝色像素点）：

![1556345601159](/1556345601159.png)

造成有两个属性可以获取的重复情况是浏览器大战的历史遗留问题，网景只支持`window.innerWidth/Height`，IE只支持 `document.documentElement.clientWidth\Height`。后来大多数浏览器都支持了`clientWidth\Height`，但是IE还是不支持`innerWidth/Height`。

虽然在桌面浏览器上这两个属性对有些迷惑，但是在手机上，还是挺有用的。

## 获取`<html>`的真实尺寸：

 They’re stored in`document.documentElement.offsetWidth` and `-Height`.

这两个属性才会被 css 的width和height影响。



# 事件的坐标

鼠标事件 给出的坐标：

1. `pageX/Y` 在 `<html>`元素中的相对位置，以 CSS pixels 为单位。
2. `clientX/Y` 在  viewport （就是浏览器窗口中的绝对位置）中的相对位置，以 CSS pixels 为单位。
3. `screenX/Y` 在你的显示器中的位置，以 Device pixels为单位。



# @media 中的关键字

```css
@media all and (max-width: 400px) {
	// styles assigned when width is smaller than 400px;
	div.sidebar {
		width: 100px;
	}
}
```

当你的设备属于all，并且 尺寸不大于 400px的时候，就应用下面的css规则。

这里的`max-width`指的是viewport还是screen呢？

1. `max/min-width/height` 的值和 `documentElement .clientWidth/Height` 中的值是一样的 (都指的是viewport)。所以单位是CSS Pixel。
2. `device-width/device-height` 的值和 `screen.width/height` 是一样的 (也就是显示屏的尺寸)。所以指的是Device Pixel。



---

# 手机浏览器上的viewport

译：[A tale of two viewports - part two](https://www.quirksmode.org/mobile/viewports2.html)

