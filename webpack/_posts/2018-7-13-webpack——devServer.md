---
layout: post
title: webpack——devServer
description: >
  devServer不会产出文件，只会将文件编译后放在内存中运行，所以我们是看不见打包输出的文件的
author: author1
image: https://cdn.jsdelivr.net/gh/zouzenghu/cdn@7.19/assets/img/webpack/webpack.jfif
---
### Webpack——devServer

### 使用devServer

* devServer不会产出文件，只会将文件编译后放在内存中运行，所以我们是看不见打包输出的文件的

```javascript
yarn add webpack-dev-server -D

//dev文件配置
devServer:{
    host: '0.0.0.0',//允许外部访问
    port:8080,//当前服务端口号
    open:true,//自动打开浏览器
    hot:true,//模块热更新
    hotOnly:true,//禁止浏览器自动刷新
    proxy:{//服务代理
   //api/users现在请求将请求代理到的请求http://localhost:3000/api/users
        '/api': 'http://localhost:3000'
    },
     publicPath: '/assets/',
    contentBase:path.join(process.cwd,'dist'),//告诉服务器从哪里提供内容
}
```

### 热更新hot

* 什么是热更新，当局部代码发生改变修改时，只对局部代码进行更新，而不是这个页面跟着刷新
1. css热更新
   
   ```javascript
   cosnt webpack=require('webpack');
   module.exports={
       devServer:{
        host: '0.0.0.0',//允许外部访问
        port:8080,//当前服务端口号
        open:true,//自动打开浏览器
        hot:true,//模块热更新
        hotOnly:true,//禁止浏览器自动刷新
        contentBase:path.join(process.cwd,'dist'),//告诉服务器从哪里提供内容
   }
       plugins:[
         new webpack.HotModuleReplacementPlugin()
       ]
   }
   ```

2. js热更新
   
   ```javascript
   //index.js
   if(module.hot){
       module.hot.accept();
   }
   ```
