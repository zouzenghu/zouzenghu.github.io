---
layout: post
title: let&const
description: >
 在 ES6 中新增了一个 let 命令用于声明变量，用法类似于 var 但是只在块级作用域中有效
author: author1
image: https://cdn.jsdelivr.net/gh/zouzenghu/cdn@7.15/assets/img/ecmascript/v2-3e43f0767c79fb0c71770127c4c3675c_1200x500.jpg
---

## let&const

### let

- 在 ES6 中新增了一个 let 命令用于声明变量，用法类似于 var    

```javascript
{
  let n = 6;
  var s = "你好";
  console.log(n);
}
console.log(s); //你好
console.log(n); //ReferenceError:n is not defined
```

## let 特性

1. 块级作用域

- 什么是块级作用域：{}一对花括号就是一个块级作用域，使用 let 声明的变量只允许在{}花括号内使用
  出了{}花括号则无效为声明，let 实际上是为 JavaScript 新增了块级作用域。

```javascript
{
  //只在该块级作用域中有效，出了{}则无效
  let a = 10;
}
//报错变量为未声明
console.log(a);
```

- 为什么需要块级作用域：ES5 中只有全局作用域和局部作用域，没有块级作用域，这导致很多时候会现莫名其妙错误

* 注意块：级作用域是不允许有返回值的

```javascript
//内层变量覆盖外层变量（声明提前）;
var n = 5;
function fun() {
  console.log(n);
  if (false) {
    var n = 10;
  }
}
fun(); //undefined
```

```javascript
//循环变量会带来变量泄露
for (var i = 0; i < 5; i++) {}
console.log(i); //4
```

2. 暂时性死区

- 只要块级作用域内存在 let 命令，它所声明的变量就绑定这个区域，不在受外部的影响，ES6 规定如果区块中存在 let 和 const 命令，则这个区块对这些命令的变量从一开始就形成封闭作用域，只要在生命前使用这些变量就会报错，使用 let 命令声明变量之前，该变量是不可用的，这称为暂时性死区（TDZ）

```javascript
var n = 5;
//绑定该块级作用域
//因为已经绑定所以在let= n变量生命前赋值会报错（不受外部影响）
if (true) {
  n = 10; //报错ReferenceError
  //TDZ（暂时性死区）
  let n; //暂时性死区结束
}
//使用typeof也会报错
typeof n; //ReferenceError
let n = 10;
```

3. 不存在变量提升

- 使用 let 声明的变量不存在声明提前，变量的使用只允许声明后使用，不允许声明前使用

```javascript
{
  console.log(a); //报错
  let a = 10;
}
```

4. 不允许重复声明

- let 命令不允许在相同的作用域内重复声明同一个变量

```javascript
{
  let n = 10;
  var n = 20; //报错
}
{
  let n = 10;
  console.log(n); //=>10
  {
    let n = 20;
    console.log(n); //=>20
  }
}
```

## const 特性

- const 声明一个只读常量一旦声明，常量的值不能修改，当然 const 只能保证栈中简单的数据类型的值无法被修改，而无法保证堆中复杂数据类型的值无法被修改，可以保证复杂类型的对象指针不被修改

```javascript
{
  //对于原始类型的数据是不允许被修改的（保存在栈中）
  const n = 10;
  n = 5; //报错TypeError
}
{
  //对于复杂类型，可以修改堆中的值，但是不允许修改保存在栈中的内存地址，保证这个对象指针是固定的
  //所以这一点要注意
  const obj = {
    a: 10,
    b: 20
  };
  obj.a = 30;
  console.log(obj.a); //=>30
  obj = {}; //报错
}
```

1. 块级作用域
2. 不允许修改栈中的值
3. 不允许重复声明
4. 存在暂时性死区（未声明前不允许使用）
5. 不存在变量提升
6. 生声明必须赋值

## let&const 应用

1. const

- const 应用一般用于保存如 PI，等一些不可变的已知的值，如果想将一个对象不可变则需要配合使用 Object.freeze 冻结属性使用

```javascript
//将一个对象完全冻结
export default function objFrozen(obj) {
  Object.freeze(obj);
  Object.keys(obj).forEach(key => {
    if (typeof obj[key] === "Object") {
      objFrozen(obj[key]);
    }
  });
}
```

2. let

- let 的应用很广泛如 for 中或者函数作用域中块级作用域中，建议能使用 let 的地方 就使用 let，当然 let 还有很有趣的特性，let 的本质是一个被自调函数包裹的变量

```javascript
//当前的i之在本轮循环中有效，每轮循环i便量都是一个新的变量，
//JavaScript内部引擎会自动的记住上一轮i循环的值，所以每轮循环i都是一个新变量，
//而for的特别之处在于（）这是父级作用域，{}这是子级作用域，
//而为什么i能正确的保存下来呢，其实就是一个闭包,每一轮的循环都会重新执行一次（），每执行一次就会
//创建出一个全新的AO，而AO中i被引用，所以AO无法被释放，i则保存下来
export default function addBtn(n = 5) {
  let frag = document.createDocumentFragment();
  for (let i = 0; i <= n; i++) {
    let btn = document.createElement("button");
    btn.innerHTML = i + 1;
    btn.addEventListener("click", e => {
      alert(i + 1);
    });
    frag.appendChild(btn);
  }
  return frag;
}
```

```javascript
//使用ES5闭包的方式模拟保存变量i
export default function addBtn(n = 5) {
  var frag = document.createDocumentFragment();
  for (var i = 0; i <= n; i++) {
    var btn = document.createElement("button");
    btn.innerHTML = i + 1;
    btn.addEventListener(
      "click",
      (function(i) {
        return function fun() {
          alert(i + 1);
        };
      })(i)
    );
    frag.appendChild(btn);
  }
  return frag;
}
```
