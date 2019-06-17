---
layout: post
title: JS模块化
description: >
 什么是模块化：将一个复杂的程序依据一定的规则，进行功能拆分，封装成几个模块（文件），并进行组合在一起，块的内部数据/实现私有化，只是向外部暴露一些结构（方法）与外部其他模块通信
author: author1
image:  https://cdn.jsdelivr.net/gh/zouzenghu/cdn@7.17/assets/img/ecmascript/u=646855610,951597263&fm=26&gp=0.jpg
noindex: true
---


### 模块化

* 什么是模块化：将一个复杂的程序依据一定的规则，进行功能拆分，封装成几个模块（文件），并进行组合在一起，块的内部数据/实现私有化，只是向外部暴露一些结构（方法）与外部其他模块通信

### 模块化进化史

1. 全局函数模式：将不同的功能封装成不同的全局函数
   
   ```javascript
   function fun(){
       console.log('fun()')
   }
   function fun1(){
       console.log('fun1()')    
   }
   //Global被污染，很容易命名冲突 
   ```

2. 简单封装：Namespace模式，将功能封装到对象当中（命名空间模式）
   
   ```javascript
   var obj={
       fun:function(){
           console.log('fun功能')
       },
       fun1:function(){
           console.log('fun1功能')
       }
   }
   //减少Global上的变量数目
   //本质是对象，一点都不安全
   ```

3. 匿名闭包：IIFE模式(立即执行函数)（闭包）
   
   ```javascript
   (function(){
       //.....代码块
       function fun(){
           console.log('fun功能')
       }
       function fun1(){
           console.log('fun1功能')
       }
   })()
   ```

4. 引入依赖：IIFE模式（增强版的IIFE模式，引入代码所需的依赖 ）
   
   ```javascript
   (function($){
       //代码块
       $('elem').click(()=>{
           console.log('模块化')
       })
   })(jQuery)//这条语句，将依赖的jQuery作为参数注入到这个代码块中
   //或者可以解释为是引入依赖
   
   //这就是现代模块化实现的基石
   ```

#### 为什么要模块化

1. 降低复杂度提高解耦性（减少复杂的程序之间的关联性）

2. 部署方便

#### 模块化带来的好处

1. 避免命名冲突（减少命名空间的污染）

2. 更好的分离，按需加载，按需引用

3. 更高的复用性

4. 高可维护

5. 降低程序的复杂度，以及程序之间的耦合度
