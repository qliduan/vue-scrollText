# scrollText （vue 2.0）
垂直方向滚动的中奖消息通知（以前都是用js的setinterval或settimeout实现，这个组件是为了尝试用css3的animation动画实现而写的，所以只兼容支持css3的浏览器，浏览器前缀只针对webkit做了兼容）
##运行效果图如下
![运行效果图](https://github.com/qliduan/vue-scrollText/blob/master/result.gif)

# Example
```html
<template>
  <div>
  		<scroll-text :data-list="dataList" item-height="20" unit="px" time="3"></scroll-text>
  </div>
</template>
<script>
	import Vue from 'vue';
	import scrollText from './scrollText';
	Vue.component('scrollText',scrollText)
	var app = new Vue({
	    el: '#app',
	    data: function() {
	        return {
	            test: 'ddd',
	            dataList:[]
	        }
	    },
	    mounted:function() {
	        this.dataList = [{
	            user:'360U74**',
	            goods:'iPad mini 4'
	        },
	        {
	            user:'fgdf74',
	            goods:'iPhone 7 plus'
	        }]
	    }
	})
</script>
```

# API
| 参数               | 描述                                                      | 类型                  | 默认值  |
|--------------------|------------------------------------------------------------------|------------------------|----------|
| dataList            | 数据消息                                  				| Array                |   |
| itemHeight          | 单条消息的高度、每次滚动的距离                          | Number |          | 
| unit         		  | 滚动高度的单位（px、rem、em）                           | String |      px    | 
| time             	  | 每次滚动的间隔时间，单位s                                | Number                 |     3    |
