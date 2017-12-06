---
title: 关于JS的模块化开发总结
date: 2017-04-10 16:33:22
categories: JS
tags:
	- JS
---

一直都在说JS的模块化开发，以前研究和使用过require之类的方法，但是一直不是很明确，今天还是在总结一下：

- **知识点一** ：AMD/CMD/CommonJs是JS模块化开发的标准；
- **知识点二** ：commonjs是用在服务器端的，同步的，如nodejs；amd, cmd是用在浏览器端的，异步的，如requirejs和seajs；
- **知识点三** ：AMD/CMD区别，虽然都是并行加载js文件，但还是有所区别，AMD是预加载，在并行加载js文件同时，还会解析执行该模块（因为还需要执行，所以在加载某个模块前，这个模块的依赖模块需要先加载完成）；而CMD是懒加载，虽然会一开始就并行加载js文件，但是不会执行，而是在需要的时候才执行。
<!-- more -->
**CommonJS**
> CommonJs 是服务器端模块的规范。
> 根据CommonJS规范，一个单独的文件就是一个模块。加载模块使用require方法，该方法读取一个文件并执行，最后返回文件内部的exports对象。
例如:
// foobar.js
``` javascript
//私有变量
var test = 123;

//公有方法
function foobar () {

    this.foo = function () {
        // do someing ...
    }
    this.bar = function () {
        //do someing ...
    }
}
```
//exports对象上的方法和变量是公有的
``` javascript
var foobar = new foobar();
exports.foobar = foobar;
```
//require方法默认读取js文件，所以可以省略js后缀
``` javascript
var test = require('./boobar').foobar;
test.bar();
```
CommonJS 加载模块是同步的，所以只有加载完成才能执行后面的操作。像Node.js主要用于服务器的编程，加载的模块文件一般都已经存在本地硬盘，所以加载起来比较快，不用考虑异步加载的方式，所以CommonJS规范比较适用。但如果是浏览器环境，要从服务器加载模块，这是就必须采用异步模式。所以就有了 AMD  CMD 解决方案。

**AMD**
> AMD 是 RequireJS 在推广过程中对模块定义的规范化产出
> AMD异步加载模块。它的模块支持对象 函数 构造器 字符串 JSON等各种类型的模块。
> 适用AMD规范适用define方法定义模块。

``` javascript
//通过数组引入依赖 ，回调函数通过形参传入依赖
define(['someModule1', ‘someModule2’], function (someModule1, someModule2) {

    function foo () {
        /// someing
        someModule1.test();
    }

    return {foo: foo}
});
AMD规范允许输出模块兼容CommonJS规范，这时define方法如下：

define(function (require, exports, module) {

    var reqModule = require("./someModule");
    requModule.test();

    exports.asplode = function () {
        //someing
    }
});
```
**CMD**
> CMD是SeaJS 在推广过程中对模块定义的规范化产出

CMD和AMD的区别有以下几点：
> 1.对于依赖的模块AMD是提前执行，CMD是延迟执行。不过RequireJS从2.0开始，也改成可以延迟执行（根据写法不同，处理方式不通过）。
> 2.CMD推崇依赖就近，AMD推崇依赖前置。
``` javascript
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
// 虽然 AMD也支持CMD写法，但依赖前置是官方文档的默认模块定义写法。
```
> 3.AMD的api默认是一个当多个用，CMD严格的区分推崇职责单一。例如：AMD里require分全局的和局部的。CMD里面没有全局的 require,提供 seajs.use()来实现模块系统的加载启动。CMD里每个API都简单纯粹。

