----
layout: post
title: webpack——splitChunks
description: >
  在开发过程中或多或少的会使用类库辅助开发，但是这些类库是不会改变的，且也不需要改变的，如果说跟业务逻辑打包在一块的话，业务逻辑是经常要改变的而引入的库是不会改变的，所以就会造成一个问题，没办法利用上浏览器的缓存机制
author: author1
image: https://cdn.jsdelivr.net/gh/zouzenghu/cdn@7.16/assets/img/webpack/webpack.jfif
---

#### webpack——splitChunks

#### 什么是代码分片

* 代码分片是，将一个大文件切割几个的小文件，比如将几个类库进行切割成单独的文件。

#### 为什么要代码分片

* 在开发过程中或多或少的会使用类库辅助开发，但是这些类库是不会改变的，且也不需要改变的，如果说跟业务逻辑打包在一块的话，业务逻辑是经常要改变的而引入的库是不会改变的，所以就会造成一个问题，没办法利用上浏览器的缓存机制，库和业务代码会被频繁的重新请求，所以要将第三方库进行单独打包，并且如果一个js文件过于庞大的话也会造成页面的阻塞了。

#### 如何实现代码分片

1. 手动代码分片
   
   ```javascript
   //缺点自动化不高
   //common.js文件
   module.exports={
       entry:{
           //将第三方类库jquery进行手动打包
   
           jquery:path.join(process.cwd(),'src/jquery.js'),
           main:path.join(process.cwd(),'src/index.js')
       },
       output:{
           filename:'./js/[name].js',
           path:path.join(process.cwd(),'dist')
   
       },
       plugins:[
           new HtmlWebpackPlugin({
               filename:'index.html',
               template:path.join(process.cwd(),'src/index.html'),
               //在进行注入到html模板中
   
               chunks:['main','jquery']
           }),
       ]
   }
   
   //jquery.js
   //在将jQuery挂载到window上
   import $ from 'jquery';
   window.$=window.jQuery=$;
   ```

2. 利用package.json分片
   
   ```javascript
   const package=require('../package.json');
   module.exports={
       entry:{
           //将项目上线依赖引入打包成一个单独文件vendor
   
           ventor:Object.keys(package.dependencies),
   
           main:path.join(process.cwd(),'src/index.js')
       },
       output:{
           filename:'./js/[name].js',
           path:path.join(process.cwd(),'dist')
   
       },
       plugins:[
           new HtmlWebpackPlugin({
               filename:'index.html',
               template:path.join(process.cwd(),'src/index.html'),
               //注入ventor
   
               chunks:['main','ventor']
           }),
       ]
   }
   ```

3. 使用splitChunksPlugin
   
   ```javascript
   //prod.js文件
   const merge=require('webpack-merge');
   const common=require('./webpack.common');
   module.exports=merge(common,{
       mode:'production',
       optimization: {
           splitChunks: {
           //默认是async只对异步代码进行分片，all对同步异步代码都分片
           //initialt同步
             chunks: 'all',
   ```

           //文件达到多大才进行分片（30kb）
             minSize: 30000,
    
           //如果分割后的文件还大于50kb则继续进行分割
    
             maxSize: 50000,
    
           //一个文件同时被几个文件引入次数才进行分割       
    
             minChunks: 1，
    
           //最多分割出多少个文件
    
             maxAsyncRequests: 5,
    
           //首屏并行最多加载多个chunks
    
             maxInitialRequests: 3,
    
           //分割出的代码命名连接符
    
             automaticNameDelimiter: '~',
    
           //分割出的块的命名长度
    
             automaticNameMaxLength: 30,
    
             name: true,
    
           //cacheGroups的作用是缓存组，它先将需要分割的块先放入缓存组中
           //当分析完毕后才进行一起分割
           //如果没有这个缓存组就没办法做到同时将两个库进行分割
    
             cacheGroups: {
    
               vendors: {
               //只对node_modules中的库进行分割
    
                 test: /[\\/]node_modules[\\/]/,
                 priority: -10,
                 filename：'[name].calss.js',
                 chunksName:name              
    
               },
    
               //如果第三方库不属于node_modules中的就走default
    
               default: {
                 minChunks: 2,
                 //权限大小
    
                 priority: -20,
                 reuseExistingChunk: true
               }
             }
           }
         }

   })

```

```
