---
title: postcss-pxtorem的使用
date: 2020-11-11 10:10:59
tags: 
  - vue
  - css
---

最近公司要开发一个H5的项目，这里用postcss-pxtorem来处理自适应的问题；
> postcss-pxtorem是PostCSS的插件，用于将像素单元生成rem单位。前端开发还原设计稿的重要性毋庸置疑，目前应用的单位最多还是rem,然而每次在制作过程中需要自己计算rem值，为了能够直接按照设计图的尺寸开发，并且能自动编译转换成rem，下面就来分享下postcss-pxtorem的使用。

## 安装依赖
```
npm install postcss-pxtorem -D
```
<!-- more -->
## 设置规则
更改postcss.config.js,该文件为使用vue-cli3自动创建的文件
```
module.exports = {
  plugins: {
    'autoprefixer': { // autoprefixer 是自动补全代码用的
      browsers: ['Android >= 4.0', 'iOS >= 7']
    },
    'postcss-pxtorem': {
      rootValue: 16,//结果为：设计稿元素尺寸/16，比如元素宽320px,最终页面会换算成 20rem
      propList: ['*']
    }
  }
}
```
## 设置根节点
首先我们要在src目录下，新建 assets/js/rem.js文件；
``` javascript
(function(c) {
  var e = document.documentElement || document.body,
    a = "orientationchange" in window ? "orientationchange" : "resize",
    b = function() {
      var f = e.clientWidth;
      // 320 默认大小16px; 320px = 20rem ;每个元素px基础上/16
      e.style.fontSize = f / 20 + "px";
    };
  b();
  c.addEventListener(a, b, false);
})(window);

```
然后在main.js中引入rem.js就可以了。
``` javascript
import "@assets/js/rem";
```

