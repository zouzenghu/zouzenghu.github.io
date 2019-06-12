---
layout: post
title: js基础——继承
description: >
 继承是OOP语言中最常见的一种概念，继承提高了代码的复用性
 以及代码的可维护性，代码的抽象和复用，极大的增加开发效率。
author: author1
image: https://cdn.jsdelivr.net/gh/zouzenghu/cdn@6.8/assets/img/ecmascript/v2-3e43f0767c79fb0c71770127c4c3675c_1200x500.jpg
noindex: true
---

说到继承我们就不得不谈一下原型链，在JS中只支持实现继承，不支持接口继承，而且其实现依赖主要是依靠原型链来实现的。其基本思想就是利用原型让一个引用类型继承另一个引用类型的方法和属性。
![Image 继承]({{cdnUrl}}/assets/img/ecmascript/v2-f85edf8f9e7640de7a72a8cd41004634_hd.jpg)
## 原型链

什么是原型链：每个构造函数都有一个原型对象，原型对象都包含一个构造函数的指针，而实例都包含一个指向原型对象的内部指针，假如我们让一个原型对象等于另一个类型的实例
，那么此时的原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含着另一个构造函数的指针，假设另一个原型又是另一个类型的实例，那么如此的层层递进，就构成了实例与原型的链条，这就是所谓的原型链基本概念。
一句话总结就是：由多级原型对象连续继承引用形成的链式结构，就是原型链。

![Image 原型链]({{cdnUrl}}/assets/img/ecmascript/prototype1.png)

## 在ES5中实现继承

```javascript
//对象间的继承

//使用API来实现继承
let father={
    car:'奔驰',
    money:'1000万',
};
let child={
    shopping(){
        console.log(`我卡上的余额${this.money}`);
    },
    driveCar(){
        console.log(`我正在开${this.car}`);
    }
};
Object.setPrototypeOf(child,father);
child.shopping();
child.driveCar();


//使用__proto__来继承
let father={
 car:'奔驰',
 money:'1000万',
};
let child={
 shopping(){
 console.log(`我卡上的余额${this.money}`);
 },
 driveCar(){
 console.log(`我正在开${this.car}`);
 }
};
//设置__proto__=father成为child的父级对象
//__proto__是浏览器的内部属性，一般不允许访问但是谷歌浏览器例外!
child.__proto__=father;

```


```javascript
//构造函数间的继承
//既继承原型对象方法，又继承构造函数属性

function father(){
    this.car='宝马';
    this.money='1000万';
}
father.prototype={
    driveCar(){
        console.log(`开车${this.car}`);
    },
    shopping(){
        console.log(`购物${this.money}`);
    }
}
function child(){
    this.pocket=0;
}
//继承一大笔钱

child.prototype=new father();

let childExample=new child();

//ok去爽

childExample.driveCar();

childExample.shopping();

console.log(childExample.car)//1000万


```
#### childExample在谷歌中查看继承__proto__属性（既继承属性又继承原型方法）
* ![img childExample]({{cdnUrl}}/assets/img/ecmascript/2019-06-12_202904_optimized.png)
* 好像比下边继承多了一层__proto__，这两个父级对象分别是new返回的新对象，以及新
  对象本身继承的prototype原型对象






```javascript
//只继承原型对象
function father(){
    this.car='宝马';
    this.money='1000万';
}
father.prototype={
    driveCar(){
        console.log(`开车${this.car}`);
    },
    shopping(){
        console.log(`购物${this.money}`);
    }
}
function child(){
    this.pocket=0;
}
//继承父对象

child.prototype=father.prototype;

let childExample=new child();

//ok爽不了了，只继承了开车和花钱的方法，却没有继承钱和车

childExample.driveCar();//开车undefined

childExample.shopping();//购物undefined

console.log(childExample.car)//undefined

```
#### childExample在谷歌中查看继承__proto__属性（只继承原型对象的实例）
* ![img childExample]({{cdnUrl}}/assets/img/ecmascript/2019-06-12_201711_optimized.png)


## ES6中的继承方式
* 在ES6中继承比ES5更加清晰和方便很多


```javascript
//不仅继承了方法而且还继承了属性
//动态方法名
let funName='fun';

class father{
    //构造函数属性
    constructor(){
        this.car='大路虎';
        this.money='2000万';
    }
    //构造函数静态方法
    static getMoney(){
        console.log('赚钱技能')
    }
    //原型对象方法
    driveCar(){
        console.log(`开车开${this.car}`);
    }
    shopping(){
        console.log(`钱包还有${this.money}`);
    }
}
class child extends father{
  constructor(){
      super('参数列表')//执行父类型构造函数，将父类型的属性，搬运到child中
  }
}
new child().car //大路虎到手了
```

## 继承的几种方式

### 借用构造函数
```JavaScript
//为了解决上述问题使用借用构造函数


```
### 组合继承
### 原型式继承
### 寄生式继承
### 寄生组合式继承





