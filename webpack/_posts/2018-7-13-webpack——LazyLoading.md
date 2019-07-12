---
layout: post
title: webpack——LazyLoading
description: >
  webpack前端代码优化，webpack推荐我们代码利用率的方式去优化代码，查看代码利用率的方式，谷歌浏览器Coverage工具
author: author1
image: https://cdn.jsdelivr.net/gh/zouzenghu/cdn@7.19/assets/img/webpack/webpack.jfif
---

### webpack——LazyLoading，Preloading，Prefetching

* webpack前端代码优化，webpack推荐我们代码利用率的方式去优化代码，查看代码利用率的方式，谷歌浏览器Coverage工具，通过谷歌工具我们就可以看到加载时代码的使用率，换而言之没用到的代码可以通过LazyLoading，Prefetching的方式去加载，主要目的是提高首屏加载时间

* 代码懒加载
  
  * 什么是懒加载，懒加载主要的目的是提高首屏的加载速度，减少用户等待时间，主要核心思想是，只加载首屏需要的核心代码逻辑，其他的一些业务代码等待核心代码加载完毕页面渲染完毕后才进行加载，或者当用户使用的时候才加载，主要实现的方式是交由import(url)引入。
  
  ```javascript
  //import引入指定，引入文件名，默认是1 num数字名，魔法注释
  //import (/*webpackChunkName:"jqeury"*/,'jqeury');
  
  //实现懒加载行为
  function getCompoent(){
      return import (/*webpackChunkName:"jquery"*/'jqueyr')
      .then(({default:$})=>{
          let element=$('<div></div>').html('Hello World');
          return element;                
      })
  }
  //使用懒加载
  document.addEventListener('click',()=>{
      getCompoent().then(element=>{
          document.body.appendChild(element[0]);
      })
  })
  ```

* webpackPrefetch & webpackLoad
  
  * 上面这种是需要用户自己执行的时候才加载文件，但是这样就造成一种情况那就是当我们加载一个代码组件时，因为网络加载延迟可能功能运行不流畅解决方法使用魔法注释，等待核心业务代码加载完毕网络空闲在加载
  
  ```javascript
  //实现懒加载行为
  function getCompoent(){
  //import(/*webpackLoad*/)跟主核心业务文件一起加载
  //import (/*webpackPrefetch:true*/'jqueyr')等待主核心代码加载完毕
  //在加载
   return import (/*webpackPrefetch:true*/'jqueyr')//推荐使用
   .then(({default:$})=>{
   let element=$('<div></div>').html('Hello World');
   return element;
   })
  }
  //使用webpackPrefetch
  document.addEventListener('click',()=>{
   getCompoent().then(element=>{
   document.body.appendChild(element[0]);
   })
  })
  ```

* 打包分析
  
  ```javascript
  //生成stats.json文件，使用webpack chart（网站）进行分析
  //文件中包含我们打包过程中产出等 所有的信息
  "scripts":{
      "dev-build":"webpack --profile --json > stats.json"
  }
  //https://alexkuz.github.io/webpack-chart/
  //https://webpack.js.org/guides/code-splitting/#bundle-analysis
  //预加载与懒加载代码优化（墙）
  ```
  
  


