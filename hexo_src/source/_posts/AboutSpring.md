---
title: AboutSpring
typora-root-url: AboutSpring
typora-copy-images-to: AboutSpring
date: 2019-05-06 20:57:00
tags:
---



**DTD文件**

用来定义一个xml文件中的**标签**有哪些，标签里面可以**嵌套**哪些其他的标签，以及标签的**属性**有哪些。

可以在书写XML文件的时候给出提示，也可以用来解析文件啥的。

> [XML](https://zh.wikipedia.org/wiki/XML)文件的**文档类型定义**（Document Type Definition）可以看成一个或者多个XML文件的模板，在这里可以定义XML文件中的元素、元素的属性、元素的排列方式、元素包含的内容等等。

由于DTD限制较多，使用时较不方便，近来已渐被[XML Schema](https://zh.wikipedia.org/wiki/XML_Schema)所取代。

**XML Schema**标准

本身的意思是“XML文件的格式/模式”，人们广义上指的是用来 限制XML文件格式 的限制文件（[XML Schema 语言](<https://zh.wikipedia.org/wiki/XML_Schema_%E8%AF%AD%E8%A8%80>)，历史上有很多种语言/标准可以做这个事情，wiki里面有个表格)。

但他也是一个**标准的名字**，“XML Schema”在2001年5月成为W3C推荐标准。由于“XML Schema”作为一种W3C的推荐标准的名字与广义的[XML Schema 语言](https://zh.wikipedia.org/wiki/XML_Schema_%E8%AF%AD%E8%A8%80)存在名称上的混淆，用户社区的一部分人采用了“WXS”来称呼它， 用户社区的另一部分人采用“**XSD**”（**X**ML **S**chema **D**efinition[首字母缩略字](https://zh.wikipedia.org/wiki/%E9%A6%96%E5%AD%97%E6%AF%8D%E7%B8%AE%E7%95%A5%E5%AD%97)）来称呼它。W3C发布的1.1标准采用了“**XSD**”作为官方称呼。

当一个实例文档针对一个schema来验证有效性时（这一过程称为*assessment*），用来验证有效性的schema可以作为参数提供给验证器，也可以在实例文档中作为两种特殊属性之一直接提供：

- `xsi:schemaLocation`
- `xsi:noNamespaceSchemaLocation`
- 这种机制要求客户启动验证以充分相信这个文档，知道文档对正确的schema是有效的。

"xsi"是名字空间"<http://www.w3.org/2001/XMLSchema-instance>"的传统前缀。

**XML 头的解释**

```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
	        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	        xmlns:context="http://www.springframework.org/schema/context"
	        xsi:schemaLocation="http://www.springframework.org/schema/beans
	            http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
	            http://www.springframework.org/schema/context
	           http://www.springframework.org/schema/context/spring-context-3.0.xsd">
	    
	
	</beans>

```

xmlns: `<bean>`标签的默认命名空间

xmlns:xsi 使用URI`http://www.w3.org/2001/XMLSchema-instance`作为一个新的命名空间的ID，然后用`xsi`作为这个ID的别名。

xmlns:context 也是声明一个新的叫 context 的命名空间，使用 `http://www.springframework.org/schema/context`作为该命名空间的唯一ID，context作为一个别名。

xsi:schemaLocation 是一个xsi命名空间的属性，定义了该`<bean>`元素中 其他的命名空间的ID和xsd文件的对应关系（一个命名空间和他对应的标签编写规则）。这个属性的格式是多个 `ID-key xsd-file-URI`键值对，中间的分隔符可以是空格或者回车。

*最后，*

<https://stackoverflow.com/questions/34202967/xmlns-xmlnsxsi-xsischemalocation-and-targetnamespace>

<https://blog.csdn.net/lengxiao1993/article/details/77914155>



## AOP

#### ClassLoader TODO

#### Concept

`Aspect` 一个类，描述了需要横向插入系统的功能

`Joinpoint` 连接点，程序执行过程中，允许插入的点，就好像粒度的单位

`Advice` 增强处理/通知，就是Aspect中实现的方法，用于增强对象的方法

`Target Object` 需要被增强的对象

`Proxy` 被增强后的对象，通常是动态生成的

`Weaving` 生成代理对象的过程

#### Weaving 的两种方式

**JDK动态代理**

> 使用`Java.lang.reflect.Proxy`类实现，该类提供`newProxyInstance(classLoader,interface[],Proxy)`方法，可以通过传入的接口列表（interface[]），生成一个包含这些方法的对象。

过程

要对`对象A`进行代理，就要生成一个`对象ProxyA`，利用`newProxyInstance`函数生成；

需要获取`对象A`的接口列表，调用函数后返回一个`Object`。

缺点

当`对象A`实现了多个接口后，返回的`Object`就不能成功转型成`对象A`的实现类，只能是`对象A`实现的多个`接口类`中的*一个*。这样调用方法就很麻烦：

> ![1558241461413](/1558241461413.png)
>
> UserDaoImpl 实现了 IDao 和 IUserDao，用Proxy产生的对象不能转型成 UserDaoImpl

**CGLIB代理**

*Code Generation Library*，第一眼还看成了CGNB。

[CGLIB](https://github.com/cglib/cglib)(*Code Generation Library*)是一个基于[ASM](http://www.baeldung.com/java-asm)的字节码生成库，它允许我们在运行时对字节码进行修改和动态生成。CGLIB通过继承方式实现代理。

这个的用法很简单，只要用`Enhancer`对象增强你想要代理的对象就好啦

```java
	public Object createProxy(Object target){
		Enhancer enhancer = new Enhancer();
		enhancer.setSuperclass(target.getClass());
		enhancer.setCallback(this);
		return enhancer.create();
	}
	@Override
	public Object intercept(Object arg0, Method arg1, Object[] arg2, MethodProxy arg3) throws Throwable {
		MyAspect asp = new MyAspect();
		asp.check();
		Object ret = arg3.invokeSuper(arg0, arg2);
		asp.log();
		return null;
	}
```



**用Spring代理**

定义一个`bean`，类为`org.springframework.aop.framework.ProxyFactoryBean`。然后设置下`property`

`proxyTargetClass`设置是否使用CGLIB，按照上面说的，如果一个实现类里面有其他的接口，当`proxyTargetClass`是`false`的时候，就会报转型失败`java.lang.ClassCastException`的错误。所以用CGLIB可以少操心。。

```xml
<bean id="userDaoProxy"
      class="org.springframework.aop.framework.ProxyFactoryBean">
    <property name="proxyInterfaces">
    	<list>
        	<value>com.example.IUserDao</value>
            <value>com.example.IDao</value>
        </list>
    <property name="target" ref="UserDaoImpl"/>
    <property name="interceptorNames" value="springAspect"/>
    <property name="proxyTargetClass" value="true"/>
</bean>
```

