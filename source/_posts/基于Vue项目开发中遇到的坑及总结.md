---
title: JavaScript代码收集
date: 2017-05-10 14:11:29
tags: 
	- JavaScript
	- Vue
---
项目在5月底启动，属于创业公司的业务扩展吧，IOS和安卓都有成型的版本，所以要做一个对应的移动端H5版的达人、KTV、派对预订，入口是微信公众号，当然少不了jssdk的使用，以及balabala的授权处理等。
## 前期准备
<hr>最初是考虑用React+Redux+Webpack，前后端完全分离，但考虑到人手不足，前后端暂时做不了完全分离，然后还有对React也不熟悉，项目时间等问题，然后就被Boss否了。
最终用了更熟悉的Vue+Vuex+Webpack。主要还是因为更轻，API更加友好，上手速度更快，加上团队里有人熟悉，可以马上开工。
比较遗憾的是因为各种原因前后端分离还不是很彻底，前端用的是smarty模板加js渲染页面。好处是首屏数据可以放到script标签里面直出，在进度条读完的时候页面就能够渲染出来了，提高首屏渲染时间。但是调试的时候十分麻烦，因为没有Node做中间层，每次都要在本地完整地跑个服务器，不然拿不到数据。
Vue，Vuex，Vue-router，Webpack这些不了解的同学就去看看<a href="https://cn.vuejs.org/v2/guide/installation.html" target="_blank" rel="external">文档</a>。MV*框架用好了真的是极大地解放生产力，特别是页面的交互十分复杂的时候。
<!-- more -->
## 项目过程中遇到的坑
<hr>
1. 遇到的第一个的坑就是transition。首页有一个滑动的banner，我是直接用css3的transition配合js定时改变transform实现的。滑动在chrome中模拟没问题，ios中没问题，但是安卓中就没有滑动，百思不得其解。起初还以为是兼容性问题，搞了好久才发现需要在css中先增加一个transform: translateX(0)，像下面一样，不然之后再通过js更改transform是没法在安卓中触发transition的。
<pre>
.slide-wp{
	transform:  translateX(0);
	-webkit-transform:  translateX(0);
	transition: transform  1.5s ease;
	-webkit-transition: transform 1.5s ease;
}
</pre>大家知道，transition的作用是令CSS的属性值在一定的时间区间内平滑地过渡。
所以个人猜测，在安卓中，当没有初始值时，translateX的改动没有被平滑地过渡，就是说transition并不知道translateX是从什么地方开始过渡的，所以也就没有平滑之说，也就没有动画了。
<hr>2. 第二个就是ES6。既然用了Webpack，当然就要配合Bebel用上ES6啦。写的时候还是很爽的。let，const，模块，箭头函数，字符串模版，对象属性简写，解构等等…但帅不过3秒，在chrome上模拟地跑一点问题都没有，一到移动端就直接白屏，页面都没有渲染出来。
排查了好久，才发现是某些扩展运算符...，某些解构和for...of...循环的问题。因为这些ES6的特性（其实不指这些）在Bebel中转换是要用到[Symbol.iterator]接口的。如下面这样。
转码前:
<pre>1.const [a, b, c, d, e] = 'hello';
2.console.log(a, b, c, d, e);//'h','e','l','l','o'</pre>
转码后:
<pre>1.'use strict';
2.var _slicedToArray = (function () { function sliceIterator(arr, i) { var _arr = []; var _n = true; var _d = false; var _e = undefined; try { for (var _i = arr[Symbol.iterator](), _s; !(_n = (_s = _i.next()).done); _n = true) { _arr.push(_s.value); if (i && _arr.length === i) break; } } catch (err) { _d = true; _e = err; } finally { try { if (!_n && _i['return']) _i['return'](); } finally { if (_d) throw _e; } } return _arr; } return function (arr, i) { if (Array.isArray(arr)) { return arr; } else if (Symbol.iterator in Object(arr)) { return sliceIterator(arr, i); } else { throw new TypeError('Invalid attempt to destructure non-iterable instance'); } }; })();
3.
4.var _hello = 'hello';
5.
6.var _hello2 = _slicedToArray(_hello, 5);
7.
8.var a = _hello2[0];
9.var b = _hello2[1];
10.var c = _hello2[2];
11.var d = _hello2[3];
12.var e = _hello2[4];
13.
14.console.log(a, b, c, d, e);//'h','e','l','l','o'</pre>
第一行先声明的_slicedToArray函数用到了[Symbol.iterator]接口，然而浏览器对这个接口的支持还很有限，特别是移动端，只有Firefox Mobile36版本以上才支持，其它清一色挂掉。
所以说ES6虽好，但真要用到实际项目中的话，还不能太激进，有些特性经过Bebel转码后性能上可能还会有所下降，所以还是应该合理地使用ES6。如果是自己折腾倒无所谓，Symbol，Class，Generator，Promise这些就随便炫技吧
<hr>3. 第三个坑就是Vue使用的问题。如其说是坑，还是不如说是我自身还不够熟悉Vue。先看一下官方说明：
<br><code>受 ES5 的限制，Vue.js 不能检测到对象属性的添加或删除。因为 Vue.js 在初始化实例时将属性转为 getter/setter，所以属性必须在 data 对象上才能让 Vue.js 转换它，才能让它是响应的。</code>
<br>当时需要在props传来的某些对象数据中增加一个是否可视属性，用来控制一个与其关联的弹出框。增加后点击视图上一点反应都没有，但是用console.log打印出来发现值的确的有变化的。
也就是说，<b>数据的变化不能触发视图更新。</b>原因就是如上面所说，因为这个属性是我后来添加的，不能被Vuejs检测到。这时候需要使用$set(key, value)这个API。
话说里面的语法需要注意下，第一个参数key是一个字符串，是一个keypath，如果假如你的数据是这样:<pre>data(){
            visitors : [{
                    "id": 1,
                    ...
                }, {
                    "id": 2,
                    ...
                }, {
                    "id": 3,
                    ...
                }],
        }
    </pre>
你需要在某次操作后为visitiors里面的每个对象增加一个show属性,则需要这样写：<pre>let str;
         for (let i = 0 , len = this.visitors.length ; i < len; i++) {
             str = "visitors[" + i + "].show";
             this.$set(str,true);
         }
    </pre>
之前真的被这东西搞了很久，明明数据变化了，视图却不更新。个人感觉新手刚使用Vue时很难发现这个问题。也怪自己对Vue，对ES5<code>getter/setter</code>的理解还不够吧。
<hr>4. IOS上CSS3动画的问题。在对img或者设置了background-image的DOM元素设置CSS动画时，动画在刚进入页面的时候有可能不被触发，需要滑动一下屏幕动画才动，安卓下则没有问题。
刚开始还以为是没有设置初始值的问题，但感觉不应该会是这样的。后来在stackoverflow上找到了解决办法(<a href="http://stackoverflow.com/questions/29219534/css-animation-not-always-starting-in-ios-8-safari" target="_blank" rel="external">戳这里</a>)。
给动画加个0.1s秒的延时<pre>animation: slide 1.5s 0.1s linear infinite;
    webkit-animation: slide 1.5s 0.1s linear infinite;
</pre>

## 关于Vuex
<hr>
Vuex 之于 vue，就相当于 Redux 之于 React。它是一套数据管理架构实现，用于解决在大型前端应用时数据流动，数据管理等问题。

因为组件一旦多起来，不同组件之间的通信和数据流动会变得十分繁琐及难以追踪，特别是在子组件向同级子组件通信时，你可能需要先\$dispatch到父组件，再\$broadcast给子组件，整个事件流十分繁杂，也很难调试。
Vuex就是用来解决这些问题的。更具体的说明可以看文档，我就不过多叙述了。我就说一下我对Vuex的一些理解。

Vuex里面的数据流是单向的,就像官方说的那样：

1. 用户在组件中的输入操作触发 action 调用；
2. Actions 通过分发 mutations 来修改 store 实例的状态；
3. Store 实例的状态变化反过来又通过 getters 被组件获知。

而且为了保证数据是单向流动，并且是可监控和可预测的，除了在mutation handlers 外，其它地方不允许直接修改 store 里面的 state。