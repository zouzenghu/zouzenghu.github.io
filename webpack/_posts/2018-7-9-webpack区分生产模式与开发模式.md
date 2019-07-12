---
layout: post
title: webpack——prod&dev
description: >
  webpack将开发模式与生产模式区分开,开发模式下没必要产出文件需但要sourceMap等调错文件，可以提高编译速度，生产模式下没必要需要devServer或sourceMap等文件，区分开生产模式与开发模式可以极大地提高项目的可维护率
author: author1
image: https://cdn.jsdelivr.net/gh/zouzenghu/cdn@7.19/assets/img/webpack/webpack.jfif
---
#### webpack区分生产模式与开发模式

1. 初始化项目
   
   ```javascript
   //初始化项目
   yarn -init -y
   //安装webpack
   yarn add webpack webpack-cli webpack-dev-server-D
   //安装webpack-merge（同Object.assign一样功能）
   yarn add webpack-merge -D
   //新建webpack.config.js文件
   new-item webpack.config.js
   ```

2. 创建目录
   
   ```javascript
   //创建config目录
   mkdir config
   //进入config目录
   cd comfig
   new-item webpack.common.js //公共webpack文件
   new-item webpack.prod.js //生产用webpack文件
   new-item webpack.dev.js //开发用webpack文件
   ```

3. package.json文件配置
   
   ```javascript
    {
        "scripts":{
            "dev":"webpack-dev-server --env=dev"//传入参数值dev
            "prod":"webpack --env=prod"//传入参数值prod
        }
    }
   ```

4. webpack.config.js文件配置
   
   ```javascript
    module.exports=env=>{
        require(`./webpack/webpack.${env}.js`)
    }
   ```

5. webpack.dev.js文件配置
   
   ```javascript
        //开发文件配置主要配置，开发环境下所需要的依赖
        const path=require('path');
        const merge=require('webpack-merge');
        const common=require('./webpack.common');
        module.exports=merge(common,{
          mode:'development',
          devtool:'cheap-module-eval-source-map',
          devServer:{
                port:8080,
                hot:true,
                open:true,
                contentBase:path.join(process.cwd(),'dist');
          }
        })
   ```

6. webpack.prod.js文件配置
   
   ```javascript
   //生产文件配置，主要配置打包文件产出时的规则
   const path=require('path');
   const merge=require('webpack-merge');
   const common=require('./webpack.common');
   const package=require('../package');
   module.exports=merge(common,{
       mode:'production',
       entry:{
           //公共库代码分片
           vender:Object.keys(package.dependencies)        
       }
   })
   ```

7. webpack.common.js文件配置
   
   ```javascript
   //公共文件配置，主要配置公共的代码功能
   const path=require('path');
   module.exports={
       entry:{
           main:'./src/index.js',
       },
       output:{
           filename:'./js/[name]_[hash].js',
           path:path.join(process.cwd(),'dist')
       }
   }
   ```
