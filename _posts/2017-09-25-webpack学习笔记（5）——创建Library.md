---
layout: default
title: git操作注意事项
comments: true
---

# webpack学习笔记（5）——创建Library

## 相关概念

### CommonJs

CommonJs是服务器端模块的规范，Node.js采用了这个规范。根据CommonJS规范，一个单独的文件就是一个模块。加载模块使用require方法，该方法读取一个文件并执行，最后返回文件内部的exports对象，属于同步加载。

### AMD

AMD是RequireJS 在推广过程中对模块定义的规范化产出，属于异步加载，适用AMD规范适用define方法定义模块。

//通过数组引入依赖 ，回调函数通过形参传入依赖 

```
define(['someModule1', ‘someModule2’], function (someModule1, someModule2) { 

    function foo () { 
        /// someing 
        someModule1.test(); 
    } 

    return {foo: foo} 
}); 
```

AMD规范允许输出模块兼容CommonJS规范，这时define方法如下： 

```
define(function (require, exports, module) { 
     
    var reqModule = require("./someModule"); 
    requModule.test(); 
     
    exports.asplode = function () { 
        //someing 
    } 
}); 
```

### CMD

CMD是SeaJS在推广过程中对模块定义的规范化产出，与AMD类似，也有所区别。

### CMD与AMD的区别

1.对于依赖的模块AMD是提前执行，CMD是延迟执行。不过RequireJS从2.0开始，也改成可以延迟执行（根据写法不同，处理方式不通过）。

2.CMD推崇依赖就近，AMD推崇依赖前置。

```
//AMD 
define(['./a','./b'], function (a, b) { 

    //依赖一开始就写好 
    a.test(); 
    b.test(); 
}); 

//CMD 
define(function (requie, exports, module) { 
     
    //依赖可以就近书写 
    var a = require('./a'); 
    a.test(); 
     
    ... 
    //软依赖 
    if (status) { 
     
        var b = requie('./b'); 
        b.test(); 
    } 
}); 
```

3、AMD的api默认是一个当多个用，CMD严格的区分推崇职责单一。例如：AMD里require分全局的和局部的。CMD里面没有全局的 require

###UMD

UMD就是同时兼容CommonJs和AMD的规范。

## 本课内容

### 外部化第三方库

在创建自己的library时，如果使用了第三方库，通常更倾向于把第三方库当作peerDependency（用户已安装好）,从而把控制权让给用户。

webpack中使用externals进行配置：

```
module.exports = {
	...
	externals: {
		lodash: {
			commonjs: 'lodash',
			commonjs2: 'lodash',
			amd: 'lodash',
			root: '_'
		}
	}
}
```

对于从一个依赖目录调用多个文件的library，无法通过externals指定目录进行移除，只能使用正则表达式逐一移除:

```
import A from 'dependency/one';
import B from 'dependency/two';

...


externals: [
  'library/one',
  'library/two',
  // Everything that starts with "library/"
  /^library\/.+$/
]
```

### 暴露library

一般来说，我们希望library能兼容不同环境，如CommonJs，AMD，Nodejs或作为全局变量。这个时候可以使用webpack的output.library属性配置：

```
output: {
	...
	filename: 'webpack-numbers.js',
	library: 'webpackNumbers'
}
```
在import 引入模块时，这可以将library bundle 暴露为名为 webpackNumbers 的全局变量。为了让 library 和其他环境兼容，还需要在配置文件中添加 libraryTarget 属性。这是可以控制 library 如何以不同方式暴露的选项。

可以通过以下方式暴露 library：

>遍历：作为一个全局变量，通过 script 标签来访问（libraryTarget:'var'）(默认配置)。
>
>this：通过 this 对象访问（libraryTarget:'this'）。
>
>window：通过 window 对象访问，在浏览器中（libraryTarget:'window'）。
>
>UMD：在 AMD 或 CommonJS 的 require 之后可访问（libraryTarget:'umd'）。

### 添加输出文件路径

在package.json中添加输出bundle文件的路径：

```
{
	"main": "dist/webpack-numbers.js"	 // 输出文件
}
```
或

```
{
  "module": "src/index.js"	// 入口
}
```