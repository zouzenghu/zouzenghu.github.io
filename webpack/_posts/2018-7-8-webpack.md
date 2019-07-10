---
layout: post
title: webpack——run
description: >
  webpack的作用是将复杂的项目分割，打包，合并，降级，来维护项目
  本质上，webpack是一个现代JavaScript应用程序的静态模块打包器当
  webpack处理应用程序时，它会递归地构建一个依赖关系图，其中包含应
  用程序需要的每个模块，然后将所有这些模块打包一个或多个bundle
author: author1
image: https://cdn.jsdelivr.net/gh/zouzenghu/cdn@7.16/assets/img/webpack/webpack.jfif
---
# Webpack
## webpack何是
* 什么是webpack
> webpack的作用是将复杂的项目分割，打包，合并，降级，来维护项目
  本质上，webpack是一个现代JavaScript应用程序的静态模块打包器当
  webpack处理应用程序时，它会递归地构建一个依赖关系图，其中包含应
  用程序需要的每个模块，然后将所有这些模块打包一个或多个bundle，它
  可以将浏览器不识别的最新语法降级为ES5或者更低的语法，webpack的定
  意就是模块打包器，将各个模块进行打包，而其他附加的功能，如语法降级
  sass编译，等都是在打包的过程中进行的

## npm命令
1. npm install module -D 将指定的包安装到指定的项目目录下 -g(全局)
2. npm uninstall name 将指定的包删除(全局) -D当前目录
3. npm install 自动安装项目中所需要的依赖

## webpack起步安装
1. npm init -y
2. npm install webpack --save
3. npm install webpack-cli --save
4. npx webpack filePath   


## webpack控制文件
1. webpack.config.js
2. package.json 
> 版本控制文件

## 全局安装与项目安装webpack
* 全局安装
> npm install webpack -g
> npm install webpack-cli -g

* 项目安装
> cd 项目目录
> npm install webpack -D
> npm install webpack-cli -D
> npm install webpack@版本号 -D  指定版本安装webpack

* webpack 命令
> npm webpack Path 这是使用全局的webpack
> npx webpack Path 这是使用当前项目中的webpack
> npx webpack -v 查看当前使用的webpack版本
> npx webpack --config filename 使用指定的webpack配置文件，进行打包

* webpack-cli的作用
> webpack-cli 可以使我们在当前项目目下命令行中使用 npx webapck 以及 webpack 等命令

* 全局安装VS项目安装
> 全局安装
  可以在全局任何地方调用webpack命令
  可以在全局任何地方使用webpack
  使用全局安装问题：所有项目都使用这一个包，如果这个包被破坏，那么所有项目都会完蛋，不同的项目之间不能
  保持其不同项目之间相对的独立性

> 局部安装
 仅可以在当前安装目录下调用webpack命令
 仅在本项目中使用webpack
 使用本地安装的好处：如果使用全局包，那么每次包的升级，更新等就会影响你的多个项目，那么依赖关系就会被破坏
 所以使用本地安装有利于不同项目之间的独立性

> 总结：
    推荐使用局部安装，这样可以保证项目与项目之间的独立性，每个项目都有自己专属的webpack，这样每个webpack的
    版本不同也不会影响到其他的项目
    注意：如果不指定配置文件，将默认使用自带的默认配置文件（非常弱只能打包js）

## 何是loader
* 什么是loader
> webpack默认是知道如何打包处理js文件，但是遇上css，png文件就不知道该怎么打包了
  loader的作用就是协助webpack打包不同类型的文件，且在打包的过程中进行，进一步的
  处理
> webpack 只要遇上无法打包的文件，就会去module中去找相应类型的loader，交给loader处理
  最后webpack在拿着loader处理好的文件进行输出
> webpack可以使用loader来预处理文件，这允许你打包除了JavaScript之外的任何静态资源
> 总结
   webpack只能处理JS类型的文件，如果有其他类型的静态资源文件需要打包，就必须使用loader，
   loader的强大之处在与，不仅可以打包，并且还可以对处理的文件，进行进一步处理，如编译sass
   等，或者将图片转为base64
   loader本身就是一个打包的方案，他知道对于某个特定的文件，webpack应该如何进行打包，webpack
   本身不知道如何处理有些文件，但是loader知道

* 核心loader
> html-loader
> url-loader
> file-loader

