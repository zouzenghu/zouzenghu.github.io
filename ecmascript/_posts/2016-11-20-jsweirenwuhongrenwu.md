---
layout: post
title: 微任务与宏任务
description: >
 我们都是知道Js分为两种任务，同步任务，异步任务，异步任务是放在任务队列中，等待有了结果后才返回主线程中执行的任务，而任务队列中又分为微任务和宏任务，这两者的区别是微任务的权限更高会先执行，也就是会插队执行，而宏任务则是按照先进先出的顺序执行。两者的区别就是任务队列中的执行顺序会不同。
author: author1
image: https://cdn.jsdelivr.net/gh/zouzenghu/cdn@7.17/assets/img/v2-3e43f0767c79fb0c71770127c4c3675c_1200x500.jpg
---
### 微任务与宏任务

#### 什么是微任务与宏任务

* 我们都是知道Js分为两种任务，同步任务，异步任务，异步任务是放在任务队列中，等待有了结果后才返回主线程中执行的任务，而任务队列中又分为微任务和宏任务，这两者的区别是微任务的权限更高会先执行，也就是会插队执行，而宏任务则是按照先进先出的顺序执行。两者的区别就是任务队列中的执行顺序会不同。

#### 微任务

* 微任务，优先级更高，并且可以插队执行，不用看定义的顺序，肯定是比宏任务先执行

* 微任务包含：promise 中的then，observer

#### 宏任务

* 宏任务优先级低，先定义的先执行遵循先进先出的队列规则

* 宏任务包含：setTimeout，setInterval，ajax，事件绑定

```javascript
setTimeout(function(){
 console.log(3)
},0);
setTimeout(function(){
    console.log(4)
},0);
new Promise(function(resolve){
    //此处是同步任务，异步任务执行一半就挂起来的原则
    console.log(1)
    resolve();
}).then(function(data){
    console.log(2)    
})
//输出1，2，3，4
```

#### 总结：

* 微任务比宏任务优先级更高，就算宏任务比微任务先进队列，那也是微任务先执行


