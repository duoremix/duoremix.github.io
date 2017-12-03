---
layout: default
title: webpack学习笔记（1）——使用webpack管理资源
comments: true
---
# webpack学习笔记（1）——使用webpack管理资源

## 相关概念

### 入口
入口是应用程序的起点，单页应用（SPA）中有一个入口，多页应用（MPA）中有多个入口。

在webpack中使用entry参数配置入口，可以传的值类型有：

```
String|Array[String]：chunk会命名为main
Object：chunk以每个key来命名
```

### 出口
出口是用于输出资源的配置，是webpack整合资源后的产物。

在webpack中使用output参数配置出口，包含的基本配置项有：

```
path：输出文件的目录路径（绝对路径）
filename：输出的文件名
```

### 模块
模块就是每一个独立的功能块，拥有应用程序最小的覆盖面。

在webpack中使用module参数配置如何模块管理，其中rules参数可用于配置匹配对应资源的规则，本课中用于配置loader。


## 本课内容

#### 了解使用script引入脚本的弊端

script标签引入脚本存在依赖关系，会导致如下问题：

```
1、无法体现脚本依赖关系
2、引入顺序错误会导致程序无法正常运行
3、无法确定脚本引入后是否有用
```

### webpack安装
使用npm进行安装即可：

```
npm install webpack
```

### bundle文件
bundle文件就是出口配置最终输出的文件，也是把项目的资源打包后整合出的脚本文件。

### webpack和gulp等打包工具的区别
gulp、grunt等打包工具的工作是把资源简单的做一些整合的工作，如压缩、丑化。

webpack在此基础上增加了对所有资源依赖的管理，创建依赖图，实现的效果是对未使用的模块避免打包。

由此看出，webpack对资源不是简单的管理，而是通过建立依赖关系对有用的模块进行管理。

#### loader

loaderd的作用是对除javascript脚本外其余类型的资源文件进行引入，常用的loader有：

```
css文件：style-loader、css-loader
注：含有css字符串的style标签，将被插入html文件的head中

图片文件、字体文件：file-loader、url-loader

csv、xml文件：csv-loader、xml-loader
```

loader配置在module对象的rules数组内，可针对不同资源文件写不同的匹配规则，从而使用不同的loader处理，如：

```
module: {
	rules: [
		{
			test: /\.css$/,
			use: ['style-loader', 'css-loader']
		}
	]
}
```