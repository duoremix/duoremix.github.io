---
layout: default
title: webpack学习笔记（2）——使用webpack管理输出与开发
comments: true
---

# webpack学习笔记（2）——使用webpack管理输出与开发

## 相关概念

### 哈希值(Hash)

在浏览器访问网站资源时，获取资源需要耗费时间，因此存在缓存机制。而在新版本部署时，需要通过更新文件名的形式通知浏览器使用新版本而不是缓存。

webpack通过output.filename进行文件名替换，其中[hash]替换可以满足以上的需求。

## 本课内容

### 管理输出

在输出文件中，若是单入口文件，一般是这样配置：

```
module.exports = {
	entry: './src/index.js',
	output: {
		filename: 'bundle.js'
	}
}
```

这种情况下output.filename使用的是静态文件名，而当多入口文件的时候，我们需要配置不同的输出文件，这个时候可以使用[name]来配置文件名：

```
module.exports = {
    entry: {
      index: './src/index.js',
      app: './src/index.js'
    },
    output: {
      filename: '[name].bundle.js'
    }
  }
```

output.filename的配置项有以下内容：
>使用入口名称：

```
filename: '[name].bundle.js'
```

>使用chunk id：

```
filename: '[id].bundle.js'
```

>加上唯一的hash值：

```
filename: '[name].[hash].bundle.js'
```

>使用基于每个 chunk 内容的 hash：

```
filename: '[chunkhash].bundle.js'
```

### HtmlWebpackPlugin

HtmlWebpackPlugin的作用是帮助我们生成一个新的html，并在html中引入新打包的文件。

安装插件：

```
npm install --save-dev html-webpack-plugin
```

在webpack中配置在plugins（插件）项中：

```
plugins: [
  new HtmlWebpackPlugin()
]
```

### CleanWebpackPlugin

CleanWebpackPlugin的作用是帮助我们把某个目录下的文件清理掉，防止遗留无用的文件。

安装插件：

```
npm install clean-webpack-plugin --save-dev
```

使用：

```
plugins: [
  new CleanWebpackPlugin(['dist'])
],
```

### source map

当webpack打包源码后，很难定位代码错误在哪个源文件，这个时候需要使用source map功能，将错误映射回源代码。

source map配置在devtool项中，这里使用inline-source-map选项：

```
devtool: 'inline-source-map'
```

注：针对官方文档提供的几种source map配置项的对比记录：

devtool配置项  | 构建速度    | 重构速度     |  是否适用生产环境 | 定位效果
--------------------|------------------|-----------------|---|---|
cheap-module-eval-source-map | 一般 | 快 | 否 | 源码（仅精确到行）
cheap-module-source-map | 一般 | 算慢 | 否 | 源码（仅精确到行）
eval-source-map | 慢 | 算快 | 否 | 源码
inline-source-map | 慢 |慢 | 否 | 源码
source-map | 慢 | 慢 | 是 | 源码



