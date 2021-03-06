---
layout: post
title: js基础——Event Loop
description: >
 事件循环规则，当主线程运行到异步任务时，异步任务执行一半就会退出主线程，主线程将异步任务挂起，主线程进行下一个任务的获取处理，如果异步任务处理完成（如发起请求，已接收到请求的）就会将处理完成的异步任务插入到任务队列的末尾，等待主线程执行完同步任务后，再去检查任务队列中的异步任务是否可以执行，如果可以执行则压入栈中执行，主线程从任务队列中读取事件，这个过程是循环不断的，所以整个的这种运行机制又称为EventLoop（事件循环）
author: author1
image: https://cdn.jsdelivr.net/gh/zouzenghu/cdn@7.18/assets/img/ecmascript/1_FA9NGxNB6-v1oI2qGEtlRQ_optimized.png
---
### Event Loop

### JavaScript线程

1. 什么是线程
   
   线程是程序中的一个执行流，每个线程都有自己专有寄存器（栈指针，程序计数器等），但代码区是共享的即不同的线程可以执行同样的函数

2. JavaScript是单线程的

3. 什么是是单线程
   
    同时只能执行一个任务，其他的任务必须等待上一个任务执行完毕才能执行入栈换句话说就是只有一个人在做事，这个人必须将手头上的事情干完才能干下面的事情

4. 什么是多线程
   
   多线程是指程序中包含多个执行流,即在一个程序中同时运行多个不同的线程来执行不同的任务,也就是说可以让几个人来干同一件事(一项任务).

5. 为什么JavaScript是单线程
   
    JavaScript执行线程与UI线程是互斥的，假设我们要操作DOM页面，一个线程在某个节点添加了一个DOM元素，而另一个线程则删除了这一个DOM元素，那么这时候浏览器该如何裁决依照那个线程为准，如果说JavaScript是多线程则会带来这一类复杂的同步问题，所以为了避免复杂性，JavaScript被设计为了单线程

6. 得出结论
   
    JavaScript中的代码都是排队执行，不会同步执行多个任务，也就是说必须等待上一个任务执行完，才可以执行下一项任务，如果某一项任务太大占用了很多时间，那么后面的任务必须等待这个任务执行完才可以执行之后的任务，这就产生页面阻塞。

7. 同步任务与异步任务
   
   JavaScript中所有任务分为两种任务，同步任务，异步任务，
   
   1. 同步任务
      
       什么是同步任务：同步任务指的是在主线程（执行栈execution content stack）上排队执行的任务，只有前一个任务执行完毕才能执行下一个任务，如一般的赋值语句，循环，分支语句等都是同步任务
      
      ```javascript
      //这些都属于同步任务，顺序执行，排队执行
      function fun(){
          console.log('我是fun')
      }
      fun();
      for(let i=0;i<10;i++){
          console.log(i);
      }
      let a=3;
      if(a>3){
          console.log('a大于三')
      }else{
          console.log('a小于三')
      }
      ```
   
   2. 异步任务
      
      什么是异步任务：如AJAX请求，定时器，DOM事件等，异步任务指的是不进入主线程，而进入任务队列（Event Loop）的任务，一旦某个异步任务处理完有结果就在任务队列中放置一个事件，当主线程中的所有任务全部执行完毕出栈，才会去检查任务队列中的异步任务是否达到执行条件，如果达到则压入栈执行（注意：是所有任务执行完毕才回去检查任务队列中的事件，这就合理解释了为什么在for中setInterval去执行，打印出i变量，i变量永远是执行完后的值，因为完全是for已经执行完了，且整个执行栈执行完了，才会去执行任务队列中的setInterval，这时再去寻找i变量那么肯定是执行完后的结果）
      
      ```javascript
      function ajax(){
          let xhr= new XMLHttpRequest();
          xhr.onreadystatechange=function(){
              if(xhr.state==200 && xhr.readyState===4){
                  return xhr.response;
              }
          }
          xhr.open('GET','www.baidu.com',true);
          xhr.send(null);//当发起请求后主线程将ajax函数出栈，并将ajax挂起
        //当ajax返回结果在再任务队列中放置一个事件，待主线程执行完其他任务回头检查，
        //并判断是否到可执行时间
      }
      console.log(ajax())
      ```
      ```javascript
      for(var i=0;i<3;i++){
          setTimeout(()=>{
            console.log(i)
          },1)
      }
      ```
   3. 总结
      
      同步任务：主线程（执行栈execution content stack）立马执行，只不过是按照代码顺序执行，执行完就弹出栈，立马执行在后面等待的下一项同步任务
      
      异步任务：主线程（执行栈execution content stack）不会立马执行，而是先放进任务队列中，当主线程中的同步任务执行完毕，才去检查这些挂起来的异步任务是否达到了执行条件，如果达到了执行条件则压入栈执行，这也就合理解释了，定时器就算达到了预设时间执行也得等待主线程中的任务执行完毕才可以执行，因为js是单线程没办法同时处理两件情
      
      假设上面定时器情况，比如我们主线程正在执行一项任务，而这项任务非常的耗时间，而这时到达了定时器预设的执行时间，这时的浏览器是先将主线程任务全部执行完毕后才会去执行定时器中的任务，也就是说不管定时器你预设的时间是多少，都必须等待主线程中的任务执行完毕才可以执行，原因就是JavaScript是单线程的没办法同时处理两项任务
      
      ```javascript
      setTimeout(()=>{
          console.log('我是异步执行函数')    
      },1);
      for(let i=0,str='';i<1000;i++){
          console.log(str+=i);    
      }
      for(let i=0,str='';i<1000;i++){
       console.log(str+=i);
       }
      //这个例子印证了，定时器执行是跟设置的时间是没有关系的
      //必须等到主程序中所有任务完成，才会去任务队列中将setTimeout回调取出并执行
      ```

### javaScript任务队列task queue

1. 什么是任务队列（task queue）
   
   任务队列是用来存放异步任务执行完有结果后的事件，如果异步任务完成，就插入到任务队列的末尾等待主线程执行（先进先出的结构）

2. 为什么要有任务队列
   
    因为JavaScript是单线程，那么就意味着所有的程序都必须排队执行，前一个任务结束才会执行下一个任务，如果说其中有一个任务非常大耗时长后面的一个任务就得一直等着，如果说没有任务队列的话，那么处理AJAX这样的异步任务就必须等待他们执行完毕有结果后才能执行下一项任务，但是这样的异步任务往往由于网络环境输出输入很慢，天知道它们什么时候会返回结果，所以JavaScript语言设计者意识到了，这种不必要的等待，设计出了任务队列这种形式来处理异步任务，当主线程运行到异步任务时，异步任务执行一半就退出主线程，挂起处于等待中的异步任务，主线程进行下一步任务的获取处理，等异步任务处理完成，就插入到任务队列末尾等待主线程处理。主线程处理完主线程中的任务，才会去检查任务队列中事件是否可执行（JavaScript事件循环机制），一来一往就抛出了另一个问题JavaScript的事件循环机制

#### JavaScript中的线程

1. JavaScript执行线程：负责执行js脚本代码

2. UI线程：负责UI展示以及页面渲染的，渲染DOM

3. JavaScript事件循环线程：负责处理异步事件处理

#### JavaScript中的事件循环机制

* 事件循环规则，当主线程运行到异步任务时，异步任务执行一半就会退出主线程，主线程将异步任务挂起，主线程进行下一个任务的获取处理，如果异步任务处理完成（如发起请求，已接收到请求的）就会将处理完成的异步任务插入到任务队列的末尾，等待主线程执行完同步任务后，再去检查任务队列中的异步任务是否可以执行，如果可以执行则压入栈中执行，主线程从任务队列中读取事件，这个过程是循环不断的，所以整个的这种运行机制又称为EventLoop（事件循环）

* JavaScript引擎如何知道任务队列中是否有满足条件等待执行的任务事件呢，能不能进入主线程执行呢，js引擎是通过不停的检查，一遍又一遍，只要同步任务主线程执行完毕，引擎就会去检查哪些挂起来的异步任务是不是可以进入主线程执行。 

* 总结：主线程只有执行完当前任务，才会去检查任务任务队列，如此反反复复的循环主线程执行完当前任务检查任务队列。

* JavaScript是单线程的，每次只能处理一件任务，一件任务处理好了才会去执行下一项任务
![img Event Loop]({{site.cdnUrl}}assets/img/js-runtime-general-model_optimized.png)