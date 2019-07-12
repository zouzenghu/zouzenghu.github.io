---
layout: post
title: webpack——babel
description: >
  什么是babel，babel的作用是将javascript代码进行语法转换，将ES6的代码转换为ES5或者更低版本的js代码
author: author1
image: https://cdn.jsdelivr.net/gh/zouzenghu/cdn@7.19/assets/img/webpack/webpack.jfif
---
### webpack——Babel

* 什么是babel，babel的作用是将javascript代码进行语法转换，将ES6的代码转换为ES5或者更低版本的js代码，大部分的浏览器都不支持ES6语法或者是对ES6语法的实现都不是很理想，而babel就是将ES6语法或者ES7语法转换为，浏览器支持比较好的版本语法。
  
  ```javascript
  //简单的配置babel
  //安装babel
  yarn add @babel/core @babel/preset-env babel-loader -D
  //babel-loader 与webpack做关联
  //@babel/core babel核心文件，将JS语法抽象为语法树
  //@babel/preset-env  自动将ES6的语法转换为ES5
  yarn add @babel/polyfill
  //ES6补偿文件
  //上边的插件只能将ES6做语法转换，但是没办法处理如Promise对象或者ES6中对象的方法，而@babel/polyfill则是补偿ES6方的填充性文件
  ```

* ```javascript
  mkdir .babelrc //新建babelrc文件
  //.babelrc配置
  {
    "presets": [
      [
        "@babel/preset-env",
        {
          "useBuiltIns": "usage",
          "corejs": "2"
        }
      ]
    ]
  }
  ```

* ```javascript
   module: {
      rules: [
        {
          test: /\.js$/,
          //忽略node_modules中的文件
          exclude: /node_modules/,
          loader: "babel-loader"
          //.babelrc文件中不能写注释，注释放在了这里
          // options: {
          //   presets: [
          //     [
          //       "@babel/preset-env",
          //       {
          //当做babel/polyfill填充时，不是将所有ES6补偿语句进行填充
  
          // 而是根据业务代码按需填充
  
          //         useBuiltIns: "usage",
          //在不声明core-js版本的时候使用useBuiltIns: "usage"会产生警告
  
          //         corejs: "2"
          //         targets: {指定浏览器版本进行语法转换
  
          //          ie: "8"
  
          //          }
  
          //       }
          //     ]
          //   ]
          // }
        }
      ]
    }
  ```

### webpack——Tree Shaking

### 什么是Tree Shaking

* 假设我们有一个模块导出两个方法add和remove，我引入了这一个模块，但是却只使用了其中的add方法，而这时在打包文件，webpack会将两个方法都打包进来但是我们却只使用了一个方法而已，所以另一个方法完全没有必要打包进来，而tree shaking的作用呢就是，帮我们处理这件事情的，只打包我们使用的方法

* 注意：tree shaking 只支持ES6 Module静态导出方式，不支持require等动态导出方式，并且，如果一个文件默认没有任何的导出，tree shaking会忽略打包，@babel/polyfill也会被忽略打包，css，scss文件也会被忽略打包。解决方法在package.json中配置一下，以下文件不使用tree shaking正常打包
  
  ```javascript
  //package.json文件配置
  "sideEffects": [
      "@babel/polly-fill",
      "*.scss",
      "*.css"
    ],
  ```

* ```javascript
   //Tree Shaking按需引入，引入那个模块就打包那个模块
   //不会引入没有使用的模块，只支持E6 Module import方式
  optimization:{
      //production模式下Tree Shaking已经默认配置好了

      usedExports: true,
  }
  ```


