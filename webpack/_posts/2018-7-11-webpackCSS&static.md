---
layout: post
title: CSS && 静态文件处理
description: >
  webpack中关于css文件处理以及，图片字体文件等静态资源处理
author: author1
image: https://cdn.jsdelivr.net/gh/zouzenghu/cdn@7.19/assets/img/webpack/webpack.jfif
---
### webpack——CSS && 静态文件处理

#### CSS（sass）处理

```javascript
    //安装开发依赖
    yarn add 
        sass-loader 
        node-sass 
        css-loader 
        postcss-loader 
        mini-css-extract-plugin 
        autoprefixer 
        uglifyjs-webpack-plugin
        -D
    //sass-loader 处理sass与webpack之间的关联
    //node-sass 编译sass
    //postcss-loader 自动添加css前缀(需要，autoprefixer来实现) postcss.config.js文件来添加插件
    //mini-css-extract-plugin  css代码分片  但是自带的css代码压缩会失效 需要optimize-css-assets-webpack-plugin来配合css压缩
    //注意：使用optimize-css-assets-webpack-plugin代码压缩时js自带的代码压缩会失效，需要uglifyjs-webpack-plugin来解决js代码压缩问题
    //css-loader 处理css中类似@import语法以及css文件的打包处理
    new-item postcss.config.js
    //postcss.config.js文件配置

    module.exports={
        plugins:[require('autoprefixer')]
    }
```
```javascript
    //webpack.common.js配置
    const miniCssExtracPlugin=require('mini-css-extract-plugin');
    module.exports={
            plugins:[
                new miniCssExtracPlugin({
                    filename:'./css/[name].css',
                    chunkFilename:'./assembly/[id].css'
                })
            ],
            module:{
                rules:[
                    {
                        test:(/\.(sc|sa|c)ss$/),
                        use:[
                            {
                            loader:miniCssExtracPlugin.loader,
                            options:{
                                  //解决css中引入图片位置错误问题（根据打包路径决定）
                                publicPath:'../',
                            }
                            },
                            {
                                loader:css-loader,
                                options:{
                                    //所有css文件或sass文件必须经过下面两个loader处理
                                    importLoaders:2,
                                }
                            },
                            'sass-loader',
                            'postcss-loader',
                        ]
                    }
                ]
            }    
    }

```
```javascript
    //webpack.prod.js文件配置
    const optimizeCssAssetsWebpackPlugin=require('optimize-css-assets-webpack-plugin');
    const UglifyJsPlugin=require('uglifyjs-webpack-plugin');
    const merge=require('webpack-merge');
    const common=require('./config/webpack.common');
    module.export=mrege(common,{
          optimization: {
                minimizer:[ 
                    //具体参数见webpack文档
                    //css代码压缩
                    //使用css代码压缩自带的js代码压缩会失效，这时会报警告
                    new optimizeCssAssetsWebpackPlugin({}),
                    //js代码压缩
                    new UglifyJsPlugin()
                ]
          }
    })
```
#### image图片处理 font字体文件处理
```javascript
//安装开发依赖
yarn add html-loader url-loader file-loader  -D
```
```javascript
    module.exports={
        rules:[
             {   //处理图片资源引用
                test:/\.(png|jpg|gif)$/,
                use:{
                    loader:'url-loader',
                    options:{
                        //如果图片大于2048就交给file-loader处理
                        //否则就转换为base64
                        limit:2048,
                        fallback:'file-loader',
                        outputPath:'./images',
                        name:'[name].[ext]'
                    }
                }
            },
            {   //处理字体文件
                test:/\.(eot|ttf|svg|woff)$/,
                use:{
                    loader:'file-loader',
                    options:{
                        name:'./fonts/[name].[ext]'
                    }
                }
            },
            {   //处理html img图片引入问题
                test: /\.(html)$/,
                use: {
                    loader: 'html-loader',
                    options: {
                        //做图片懒加载时，使用data-src="url"将图片打包
                        attrs: ['img:src', 'img:data-src', 'audio:src'],
                        minimize: true
                    }
                }
            }
        ]
    }
```