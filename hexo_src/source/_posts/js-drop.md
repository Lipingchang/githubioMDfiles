---
title: js-drop
typora-root-url: js-drop
typora-copy-images-to: js-drop
date: 2019-07-22 18:16:12
tags:

---



### 用 [ ] 把变量 变成Object 的key

```javascript
let myCusKey = 'myKey';
let myCusValue = 'myValue';
let obj = {
  [myCusKey]: myCusValue,
}
console.log(obj); // { myKey: 'myValue' }
```





# Hoisting

https://developer.mozilla.org/en-US/docs/Glossary/Hoisting

变量提升.

Conceptually, for example, a strict definition of hoisting suggests that variable and function declarations are physically moved to the top of your code, but this is not in fact what happens. Instead, the variable and function declarations are put into memory during the *compile* phase, but stay exactly where you typed them in your code.

严格意义上, 变量提升会把**函数**和**变量**的声明, 物理(手动)地移动到整段代码的前面, 但实现上并不如此. 在编译阶段, 变量和函数的声明被(先?)放入内存, 而不会把他们从你输入的代码中移除. 

```javascript
console.log(a); // undefined
let a;
a = '1';
```

上述例子, let a; 提升了, 但是 a = '1'; 没有, 只有在执行到这句之后才会赋值.





# 类

类的声明: 用`class`关键字 | 用类表达式

```javascript
/* 命名的类 */
class Rectangle {
	constructor(height, width) {
		this.height = height;
		this.width = width;
	}
}
/* 匿名的类 */ 
let Rectangle = class {
  // ... 
}
```

**函数声明** 和 **类声明** 之间的重要区别是: 函数声明会**提升,** 类需要**先定义**, **后使用**. 不然会 得到 `ReferenceError`

### 类主体/类方法的定义 ###

类声明(命名类) & 类表达式(匿名类) , 类体中的函数都会在**严格模式**下执行. ( e.g., 构造函数, 静态方法, 原型方法, getter, setter ).

constructor 方法用于创建和初始化. 

super关键字  TODO 没有介绍

```
/
```





# 迭代函数 生成器

`function*` 定义了一个生成器的函数, 会返回一个 Generator 对象.

调用 `generator` 的 `next` 方法, 就可以获取: `{value: undefined, done: true}`, 格式的返回值, 

```javascript
function* generator(i) {
  yield i;
  yield i + 10;
}
var gen = generator(10);
gen.next().value // 10
gen.next().value // 20
```

`yield*` 和 `yield`, 加了星号 (asterisk) 后, 可以把next向里面的生成器传递.

 ```javascript
function* g1() {
	yield 1;
	yield 2;
	yield 3;
}
function* g2() {
	yield 'a';
	yield* g1();
  yield* ['c','d','e']
	yield 'f';
}
 ```

// togo https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*