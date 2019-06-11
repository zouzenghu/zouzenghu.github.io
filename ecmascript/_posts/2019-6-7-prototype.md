---
layout: post
title: js基础——继承
description: >
 继承是OOP语言中最常见的一种概念，继承提高了代码的复用性
 以及代码的可维护性，代码的抽象和复用，极大的增加开发效率。
author: author1
image: https://cdn.jsdelivr.net/gh/zouzenghu/cdn@6.0/assets/img/ecmascript/v2-f85edf8f9e7640de7a72a8cd41004634_hd.jpg
noindex: true
---
说到继承我们就不得不谈一下原型链，在JS中只支持实现继承，不支持接口继承，而且其实现依赖主要是依靠原型链来实现的。其基本思想就是利用原型让一个引用类型继承另一个引用类型的方法和属性。

## 原型链
什么是原型链：每个构造函数都有一个原型对象，原型对象都包含一个构造函数的指针，而实例都包含一个指向原型对象的内部指针，假如我们让一个原型对象等于另一个类型的实例
，那么此时的原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含着另一个构造函数的指针，假设另一个原型又是另一个类型的实例，那么如此的层层递进，就构成了实例与原型的链条，这就是所谓的原型链基本概念。
一句话总结就是：由多级原型对象连续继承引用形成的链式结构，就是原型链。

![w3m Screenshot](https://cdn.jsdelivr.net/gh/zouzenghu/cdn@6.0/assets/img/ecmascript/prototype1.png){:data-width="1920" data-height="1260"}
        原型链图解
{:.figure}

在ES5中使用的prototype来实现继承
```
Object.setPrototypeOf(child,father)

```
## 继承的几种方式

### 寄生式继承





