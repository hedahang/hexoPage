---
title: 关于vue中mixins混入的用法
date: 2020-06-22 16:26:10
categories: JS
tags:
  - JS  
  - VUE
---

最近项目中要用到很多eCharts图表组件，需要在窗口尺寸发生变化时，重置图表的大小，此时如果在每个组件里面都去实现一段监听代码，代码重复太多了，此时就可以使用混入来解决这个问题，mixins是Vue提供的一种混合机制，用来更高效的实现组件内容的复用。


# JS代码：
``` javascript
// 混入代码
import { debounce } from "lodash";
const resizeChartMethod = Symbol();

export default {
  data() {
    // 在组件内部将图表init的引用映射到chart属性上
    return {
      myChart: null,
      [resizeChartMethod]: null
    };
  },
  created() {
    var _this = this;
    this[resizeChartMethod] = debounce(function() {
      if (_this.myChart) {
        _this.myChart.resize();
      }
    }, 100);
    window.addEventListener("resize", this[resizeChartMethod]);
  },
  methods: {},
  beforeDestroy() {
    window.removeEventListener("reisze", this[resizeChartMethod]);
  }
};
```
<!-- more -->
# 图表组件代码
``` html
<template>
  <div class="chart"></div>
</template>
```
``` javascript
import echartMixins from './echarts-mixins'
export default {
  // mixins属性用于导入混入，是一个数组，数组可以传入多个混入对象
  mixins: [echartMixins],
  data() {
    return {
      chart: null
    }
  },
  created() {
    this.chart = echarts.init(this.$el)
  }
}
```