---
layout: post
title: webpack——sourceMap
description: >
  sourceMap是webpack中一个源码映射文件，它主要的作用是帮助我们调试bug使用的，sourceMap会自动生成一个文件将我们的源码映射进去
author: author1
image: https://cdn.jsdelivr.net/gh/zouzenghu/cdn@7.16/assets/img/webpack/webpack.jfif
---

### 什么是sourceMap

* sourceMap是webpack中一个源码映射文件，它主要的作用是帮助我们调试bug使用的，sourceMap会自动生成一个文件将我们的源码映射进去

### 为什么使用sourceMap

* 如果在开发中不使用sourceMap的话调错将变得异常困难，因为我们需要在编译好做完处理的代码中调错，使用sourceMap就可以直接知道源码哪里出现了错误

### 如何使用sourceMap

```javascript
devtool:'cheap-module-eval-source-map',//在开发模式中推荐使用
devtool:'cheap-module-source-map',//在生产环境中使用
```

1. source-map
   
   * 会生成map格式的文件,大而全提示信息详细
     
     ```javascript
     devtool:'source-map'
     ```

2. inline-source-map
   
   * 不会生成map格式文件，包含映射关系的代码会放在打包后生成的代码中
     
     ```javascript
     devtool:'inline-source-map'
     ```

3. inline-cheap-source-map
   
   * cheap两种作用：一是将错误只定位到行，不定位到列，二是映射业务代码，不映射loader和第三方库等，会提升打包构建速度
     
     ```javascript
     devtool:'inline-cheap-source-map'
     ```

4. inline-cheap-module-source-map
   
   * module会映射出loader和第三方库
     
     ```javascript
     devtool: 'inline-cheap-module-source-map',
     ```

5. eval
   
   * 用eval的方式生成映射关系代码，效率和性能最佳，但是当代码复杂时提示信息不精确
     
     ```javascript
     devtool: 'eval',
     ```


