---
title: 关于Markdown的基本用法
date: 2018-07-11 12:16:10
categories: Markdown
tags:
  - Markdown  
---


Markdown 是一种文本格式。你可以用它来控制文档的显示。使用 markdown，你可以创建粗体的文字，斜体的文字，添加图片，并且创建列表 等等。基本上来讲，Markdown 就是普通的文字加上 # 或者 * 等符号。

### 语法说明
#### 标题
```html
# 这是 <h1> 一级标题
## 这是 <h2> 二级标题
### 这是 <h3> 三级标题
#### 这是 <h4> 四级标题
##### 这是 <h5> 五级标题
###### 这是 <h6> 六级标题
```
#### 强调
```html
*这会是 斜体 的文字*
_这会是 斜体 的文字_

**这会是 粗体 的文字**
__这会是 粗体 的文字__

_你也 **组合** 这些符号_

~~这个文字将会被横线删除~~
```
-----------
<!-- more -->
#### 列表
##### 无序列表
```html
- Item 1
- Item 2
  - Item 2a
  - Item 2b
```
##### 有序列表
```html
1. Item 1
1. Item 2
1. Item 3
   1. Item 3a
   1. Item 3b
```
#### 添加图片
```html
![GitHub Logo](/images/logo.png)
Format: ![Alt Text](url)
```
测试图片：
![GitHub Logo](/uploads/author.jpg)
#### 链接
```html
https://github.com - 自动生成！
[GitHub](https://github.com)
```
#### 引用
```html
正如 Kanye West 所说：

> We're living the future so
> the present is our past.
```
#### 分割线
```html
如下，三个或者更多的:

连字符: ---

星号: ***

下划线: ___
```
#### 行内代码
```html
我觉得你应该在这里使用
`function(){console.log('行内代码')}` 
才对。
```
#### 代码块
你可以在你的代码上面和下面添加 ``` 来表示代码块
##### 语法高亮
代码块可以添加任何一种语言的语法高亮，如：
```javascript {.class1 .class}
function add(x, y) {
  return x + y
}
```
##### 代码行数
如果你想要你的代码块显示代码行数，只要添加 line-numbers class 就可以了。例如：
```javascript {.line-numbers}
function add(x, y) {
  return x + y
}
```
你可以通过添加 highlight 属性的方式来高亮代码行数，例如：
```javascript {.line-numbers, highlight=[1,3-4]}
function add(x, y) {
  return x + y
}
var number = add(10,20);
```
#### 任务列表
```html
- [x] @mentions, #refs, [links](), **formatting**, and <del>tags</del> supported
- [x] list syntax required (any unordered or ordered list supported)
- [x] this is a complete item
- [ ] this is an incomplete item
```
- [x] @mentions, #refs, [links](), **formatting**, and <del>tags</del> supported
- [x] list syntax required (any unordered or ordered list supported)
- [x] this is a complete item
- [ ] this is an incomplete item
#### 表格
```html
First Header | Second Header
------------ | -------------
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column
```
First Header | Second Header
------------ | -------------
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column

### 扩展的语法
#### Emoji & Font-Awesome
只适用于 markdown-it parser 而不适用于 pandoc parser。 缺省下是启用的。你可以在插件设置里禁用此功能。
```html
:smile:
:fa-car:
```
:smile:
:fa-car:
#### 上标
```html
30^th^
```
30^th^
#### 下标
```html
H~2~O
```
H~2~O
#### 脚注
```html
Content [^1]
[^1]: Hi! This is a footnote
```
Content [^1]
[^1]: Hi! This is a footnote
#### 标记
```html
==marked==
```
==marked==

<span>213123</span>