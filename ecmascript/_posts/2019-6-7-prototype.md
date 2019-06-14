---
layout: post
title: js基础——继承
description: >
 继承是OOP语言中最常见的一种概念，继承提高了代码的复用性
 以及代码的可维护性，代码的抽象和复用，极大的增加开发效率。
author: author1
image: https://cdn.jsdelivr.net/gh/zouzenghu/cdn@7.15/assets/img/ecmascript/v2-3e43f0767c79fb0c71770127c4c3675c_1200x500.jpg
noindex: true
---

说到继承我们就不得不谈一下原型链，在JS中只支持实现继承，不支持接口继承，而且其实现依赖主要是依靠原型链来实现的。其基本思想就是利用原型让一个引用类型继承另一个引用类型的方法和属性。
![Image 继承]({{ site.cdnUrl }}assets/img/ecmascript/v2-f85edf8f9e7640de7a72a8cd41004634_hd.jpg)

## 原型链

什么是原型链：每个构造函数都有一个原型对象，原型对象都包含一个构造函数的指针，而实例都包含一个指向原型对象的内部指针，假如我们让一个原型对象等于另一个类型的实例
，那么此时的原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含着另一个构造函数的指针，假设另一个原型又是另一个类型的实例，那么如此的层层递进，就构成了实例与原型的链条，这就是所谓的原型链基本概念。
一句话总结就是：由多级原型对象连续继承引用形成的链式结构，就是原型链。

![Image 原型链]({{ site.cdnUrl }}assets/img/ecmascript/prototype1.png)

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

* ![img childExample]({{ site.cdnUrl }}assets/img/ecmascript/2019-06-12_202904_optimized.png)

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

* ![img childExample]({{ site.cdnUrl }}/assets/img/ecmascript/2019-06-12_201711_optimized.png)

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

### 组合式继承

```javascript
//在子级构造中使用call，apply来改变父级构造的this指向
function father(name){
    this.name=name;
}
father.prototype={
    init(){
        console.log(this.name);
    }
}
function child(){
     //借用构造函数继承属性
    father.call(this,'小明');
}
//弥补重写原型丢失构造函数属性
child.constructor=child;
Object.setPrototypeOf(child.prototype,father.prototype);
//注意：定义子级构造子级的原型方法时
//不可以使用=赋值的方式来给原型对象赋值
//必须使用.在原型对象上在创建一个方法
//如果使用=号会将子级原型对象本身，继承父级对象的指针，给覆盖掉，造成继承失效
/*  错误示例
    child.prototype={
        printing(){
            this.init()
        }
    }
*/
child.prototype.printing=function(){
  this.init();
}
```

### 原型式继承

```javascript
//原型继承的基本思想是，借助原型可以基于现有的原型对象创建新对象
//并在返回的新对象中在进行扩展
function createObject(obj){
    function F(){};
    //注意：这只是一个浅拷贝不是深拷贝
    F.prototype=obj;
    
    return new F();    
    
}

let obj={
    name:['小明','小张','小李']    
}

let child=createObject(obj);

child.name.push('小宋');

console.log(child.name);//['小明','小张','小李','小宋']

console.log(obj.name);//['小明','小张','小李','小宋']


//这种继承方式要求必须有一个对象作为另一个对象的基础，如果有这么一个对象的话
//可以把它传递给object()函数，然后再根据需求对得到的对象加以修改即可
//在这个例子中我们将obj传入crateObject,然后该函数就会返回一个新对象，这个新
//对象将obj作为原型，新对象共享着obj中的属性和方法

//在ES5中规范了这种继承方法，使用Object.create(obj,{扩展属性})
//只不过麻烦的一点扩展属性要指定四大属性
let father={
    name:['小明','小花','小张']
}
let child=Object.create(father,{
   init:{
        value:function(){
            this.name.push('小宋')
            console.log(this.name)
        },
                       
   }
})
```



### 寄生式继承

```javascript
//寄生式继承是与原型式继承紧密相关的一种思路
function createObject(obj){
 function F(){};
 F.prototype=obj;
  return new F();
 }
function create(obj){
    let clone= Object(obj);
    //基于返回的新对象进行属性扩展或者方法扩展
    clone.init=function(){
        console.log(this.name);
    };
    return clone;
}
let father={
    name:['小明','小花','小张']
}
let child=create(father);
child.init();
```

### 寄生组合式继承

```javascript
function createObject(obj){
     function F(){};
     F.prototype=obj;
     return new F();   
}
function create(child,father){
    let prototype=createObject(father.prototype);
    //弥补因重写原型对象而失去默认的constructor属性
    prototype.constructor=child;
    child.prototype=prototype;    
}
function father(val){
    this.name=['小明','小李','小张',val];    
}
father.prototype={
    init(){
        console.log(this.name);
    }
}
function child(){
    father.call(this,'小宋')
}
create(child,father);
child.prototype.printing=function(){
    this.init();
}
new child().printing();
```

### 最实用的继承方式

```javascript
//上面的方式都是比较老的继承方式，而且不够清晰语义化
//假设我们现在定义两个个方法模拟车
//功能：两种方法中都有前进，倒车，启动 
//不同的功能是
//Benz: 座椅按摩,
// landRover:差速锁，

//抽象出一个公共的对象
class move{
    constructor(name,speed){
        this.name=name;
        this.speed=speed;
    }
    forward(){
        console.log(`${this.name}以时速${this.name}公里在前进`);
    }
    backOff(){
        console.log(`${this.name}以时速${this.speed}公里在倒车`)        
    }
    start(){
        console.log(`${this.name}启动车辆`)         
    }
}
class benz extends move{
        constructor(){
            super('奔驰','200');
        }
        massage(){
            console.log('奔驰车启动座椅按摩功能')
        }
}
class landRover extends move{
    constructor(){
        super('路虎','180')
    }
    Differential(){
        console.log('路虎启动差速锁功能')
    }    
}

let Tom= new benz();//恭喜小明喜提奔驰一辆
Tom.start()//启动奔驰
Tom.forward()//奔驰以200时速前进
Tom.massage()//启动奔驰座椅按摩功能

let Jack= new landRover();//恭喜小红喜提路虎一辆
Jack.start()//启动路虎
Jack.Differential()//启动路虎差速锁功能

//当然真正开发的时候不是这样做的，上面只是一种使用继承的例子
//在真正开发中，我们要将各个模块功能划分抽象，这才是最难的地方
//以模块化的方式管理项目，各个模块各司其职降低耦合，提升内聚
//最后在根据需求在主文件中组装各个模块，我个人理解这才是王道
//既能复用代码，又能很好的管理项目
```

### ![img childExample]({{ site.cdnUrl }}assets/img/ecmascript/prototype2.png)

### 小知识点：

* 原型链查找规则：只可  子——>父 不可  父——>子 找到则不再继续向上查找如果没找到才延作用域链继续向上查找，没找到返回undefined

* 原型链属性方法使用原则：先自有后共有，找到则不再继续向上查找

* 什么时候在原型链上查找：只要 .属性 点出的属性或   .方法都在原型链中查找

* js中所有的数据类型都继承自Object，所以所有数据类型都可以使用以下方法

* .valueOf()  .toString() 等方法，只不过在不同的属性类型下使用这些方法可能产生的结果不同，因为大部分的类型对象都重写了自身的这种方法

* 如果重写了怎么调用父级的原型对象的方法呢                                                                  

* ex：如toString方法  Object.prototype.toString.call(obj，参数列表)使用call改变this强行调用
  
  

### 总结：

* 什么是继承：子对象不用创建也可以直接使用（讲白了就是复用代码）

* 何时用继承：只要一类子对象，需要相同的属性或功能时，都要将相同的属性和功能在父对象中定义一次即可

* 为什么继承：即解决空间又复用代码（解决了copy带来的问题）

* 使用属性查看继承:  .prototype 构造指向原型   .constructor原型指向构造     

* __proto__子级指向父级原型对象（内部属性）

* 使用API获得父级原型对象：super ES6中     Object.getPrototypeOf(child)ES5中








