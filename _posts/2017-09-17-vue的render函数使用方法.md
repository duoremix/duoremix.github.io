---
layout: default
title: vue的render函数使用方法
comments: true
---
# vue的render函数使用方法

### 使用template的短板

目前我们使用vue开发组件，多数都是通过写template的方法去写html，如：

```
<template>
	<div>hello world</div>
</template>
```
然而在某些情况下，我们需要用到js的特性去帮助我们渲染html，假如我们要实现一个根据类型渲染不同组件的功能，你可能会这样去做：

```
my-component组件:

<template>
	<div>
		<a-component v-if="type == 'a'"></a-component>
		<b-component v-else-if="type == 'b'"></b-component>
		<c-component v-else-if="type == 'c'"></c-component>
	</div>
</template>

调用：
<my-component type="a"></my-component>

```
这样的写法比较不好，首先它的代码比较冗长，其次，当我们需要增加新的组件时，也需要手动去写html代码。这个时候我们就需要用到render函数了。

### 什么是render函数
render函数是vue提供的一个全局方法，它可以让我们使用js的写法渲染我们的模板，实现更灵活的逻辑。下面我们用render方法来解决上面的问题：

```
let components = ['a-component', 'b-component', 'c-component']
let type = 'a'
Vue.component('my-component', {
  render: function (h) {
   let result = ''
	for (i in components) {
		if (components[i] === type) {
			result = components[i]
			break
		}
	}
	return h(result)
})
```
这里实际上把我们写在v-if里面的判断逻辑转化成了用js去实现，从而成为一个公用的渲染方法，这样我们只需管理components这个数组即可完成组件的管理。

实际上，上文中的h参数是一个惯用简写，它真正的参数名叫createElement，可以传的参数如下（参考官方文档）：

```
// @returns {VNode}
createElement(
  // {String | Object | Function}
  // 一个 HTML 标签字符串，组件选项对象，或者一个返回值类型为 String/Object 的函数，必要参数
  'div',
  // {Object}
  // 一个包含模板相关属性的数据对象
  // 这样，您可以在 template 中使用这些属性。可选参数。
  {
    // (详情见下一节)
  },
  // {String | Array}
  // 子节点 (VNodes)，由 `createElement()` 构建而成，
  // 或简单的使用字符串来生成“文本节点”。可选参数。
  [
    '先写一些文字',
    createElement('h1', '一则头条'),
    createElement(MyComponent, {
      props: {
        someProp: 'foobar'
      }
    })
  ]
)
```