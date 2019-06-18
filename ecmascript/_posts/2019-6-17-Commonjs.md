---
layout: post
title: Commonjs，AMD，CMD(模块化规范)
description: >
 commonJS,AMD,CMD是一个模块化标准，主要的实现者是nodeJs，主要利用exports导出模块，require()引入模块，形成模块化方法
author: author1
image: https://cdn.jsdelivr.net/gh/zouzenghu/cdn@7.17/assets/img/ecmascript/u=646855610,951597263&fm=26&gp=0.jpg
---


### commonJS模块规范

#### 什么是commonJs规范

* commonJS是一个模块化标准，主要的实现者是nodeJs，主要利用exports导出模块，require()引入模块，形成模块化方法
* 每一个文件都可以当做一个模块，在服务器端，模块加载是运行时同步加载的
* 在浏览器端，模块需要提前编译打包处理

#### 基本语法

```javascript
//暴露模块两种,暴露的本质都是exports对象，在没暴露之前exports对象是一个空对象
//而暴露之后就将exports对象进行了一个赋值替换
module.exports=value;
exports.xxx=value;
//引入模块
let name=require('../路径')
//自定义模块
require('模块文件路径')
//第三方模块
require('模块名')

//注意使用：exports.name可以无限的暴露出所需的模块，因为是向exports对象中创建新的属性

//使用exports=val；这种方式之能暴露一次，这是一条赋值语句，新的会将旧的模块覆盖掉
```

#### 实现

* 服务器端实现

* 1. node.js
  
  2. http://nodejs.cn/

* 浏览器端实现

* 1. Browserify
  
  2. http://browserify.org/
  
  3. 第三方的模块打包工具如webpack

#### 基于commonjs实现浏览器端模块化

```javascript
 //Browserify 模块化使用教程
 //目录结构
     //dist
     //src
       //app.js
     //index.html

 //1.下载browserify
     //注意：这两个文件都必须下载
     //全局：npm install browserify -g     
     //局部：npm install browserif --save-dev
 //2.打包运行
     // browserify js/src/app.js -o js/dist/bundle.js
     //将src下的app.js打包输出到dist目录下bundle.js
     //-o => output
 //页面使用直接引入打包后的js
```

### AMD 模块规范

#### 什么是AMD规范

* 1. Asynchronous Module Definition(异步加载模块 )
  
  2. 专门用于浏览器端，模块的加载是异步的 

#### 基本语法

```javascript
//定义暴露模块
//定义没有依赖的模块
define(function(){
    return 模块
})
//定义有依赖的模块，注入依赖的模块并声明形参使用
define(['依赖模块一','依赖模块二'],function(m1,m2){
    return 模块;
})

//引入使用模块,将这个模块所依赖的模块注入，然后在回调函数中执行
require(['依赖模块一','依赖模块2'],function(m1,m2){
    使用m1/m2
})
```

#### 实现浏览器端

* 1. Require.js
  
  2. http://www.requirejs.cn/

```javascript
//不使用AMD，进行模块化规范
//car.js
    //定义一个没有依赖的模块
    (function(window){
        let car='路虎';
        function getCar(){
            return car;
        }
        //将方法暴露出去
        window.car={getCar};
     })(window)        
//alerts.js
    //定义一个有依赖的模块
    (function(window,car){//使用形参接收getCar
        let name='小红';
        //shopping函数的执行依赖于car.getCar
        function shopping(){
            alert(`${name}开着新买的${car.getCar}去购物！`);
        }
        window.alerts={shopping};        
    })(window,car)//将car.getCar方法注入


//App.js
    //<script src='car.js'></script>
    //<script src='alerts.js'>
    //引入以上的两个模块
    (function(alerts ){
        //使用模块
        alerts.shopping();
    })(alerts);
//没有前端自动化工具帮忙的话带来的问题
//1，http请求会随着模块的数量而增多
//2，还需注意各个模块之间的依赖关系来，引入js文件
//3，难以维护  


//使用模块化规范AMD实现模块化
    //1，下载require.js，并引入
    //2，将require.js导入项目：js/libs/require.j
    //3，创建项目结构
    //js
        //libs
            //requore.js
    //modules
        //car.js
        //alerts.js
    //main.js
    //index.html

//使用require.js
    //car.js
        //定义没有依赖的模块
        define(function(){
           let car='路虎';
           function getCar(){
               return car;
           }        
         })
        //暴露模块
        return {getName};
    //alerts.js
        //定义有依赖的模块
        define(['car'],function(car){
             let name='小红';
             //shopping函数的执行依赖于car.getCar
             function shopping(){
                 alert(`${name}开着新买的${car.getCar}去购物！`);
              }
             //暴露模块
             return {shopping};
        });
     //汇总到主模块中
         (function(){
             requirejs.config({
               //baseUrl:'基本路径，指定从那个文件开始找下方的所依赖的文件'
                 //映射文件路径
                 paths:{
                 //注意：不用加后缀js，require会自动帮我们加上js后缀
                    car:'./modules/car',
                    alerts:'./modules/alerts' 
                   //jquery支持AMD标准   
                    jquery:'./modules/jquery'                                     
                 },
                 //如果第三方库不支持AMD标准
                 shim:{
                    Vue:{
                        exports:'Vue',
                    }                     
                 }
             })           
             requirejs(['alerts','jquery','Vue'],function(alerts){
                 alerts.shopping();//直接调用
                 $('body').css('background','red');
             })
         })

    //html中引入入口文件

//<script data-main='js/main.js' src='js/libs/requore.js'></script>
//data-main文件指向的是我们的主文件，src引入的是require文件库
//剩下的模块require都会自动帮我们引入
```

### CMD 模块规范

#### 什么是CMD规范

* 1. 专门用于浏览器端，模块的加载是异步的
  
  2. 模块使用时才会加载执行
  
  3. Common Module Definition （通用模块定义）

#### 基本语法

```javascript
//定义没有依赖的模块
define(function(require,exports,module){//注意上边的三个形参必须写
    exports.xxx=value;
    module.exports=value;    
})
//定义有依赖的模块
define(function(require,exports,module){
    //引入依赖模块（同步）
    var module2= require('./modules')
    //引入依赖模块（异步）
    require.async('./modules',function(modules){

    })
    //暴露模块
    exports.xxx=name;    
})

//引入使用模块
define(function(){
    var m1= require('./module1');
    var m2= require('./module2');
    m1.xxx();
    m2.xxx();
})
```

#### 实现浏览器端

* 需要依赖库 Sea.js(它的官网已被出售)
  
  ```javascript
  //1，下载require.js，并引入
  //2，将require.js导入项目：js/libs/require.j
  //3，创建项目结构
  //js
      //libs
          //sea.js
      //modules
          //car.js
          //alerts.js
          //main.js
  //index.html
  
   //定义模块
   //car.js
   //没有依赖的模块定义
        define(function(require,exports,module){
            let car='路虎';
            function getCar(){
                return car;
            };
            module.exports={getCar}; 
        })
   //alerts.js
   //有依赖的模块定义
       define(function(require,exports,module){
          let name='小红';
            //shopping函数的执行依赖于car.getCar
          //同步引用
          let car=require('./car');
          function shopping(){
              alert(`${name}开着新买的${car.getCar}去购物！`);
           }
           //异步引用
          // require.async('./car',function(car){
             // function shopping(){
               //   alert(`${name}开着新买的${car.getCar}去购物！`);
              // }             
          // })
           exports.alerts={shopping}
  
       })
     //main.js
     define(function(){
         let shopping= require('./alerts.js');
             shopping();
     })
     //html中引入文件
     //<script src='js/libs/sea.js'></script>
     //<script>
         //seajs.use('./js/modules/main.js');
     //</script>
  ```

#### 总结

* CommonJs，CMD，AMD，它们都是模块打包的一种规范，但是使用这几种方式进行打包会非常的麻烦，不够自动化，所以实际中不会使用只是了解即可，在实际中通常都是webpack，配合ES6的export 和 import 语法进行模块化开发，当然模块化开发中还有很多原则，如资源就近，模块所需的静态资源放在一个目录文件夹下，将公共的模块进行单独抽离等。


