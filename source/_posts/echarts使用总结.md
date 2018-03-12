# echarts使用总结
title: echarts使用总结
tags: 前端
date: 2018-01-29
toc: true

---
项目中需要实现数据可视化，在前辈的推荐之下，最终选取了echarts来实现，在此关于echarts的使用进行总结，最终代码分享至[我的github](https://github.com/Yabea/front/tree/master/learn%20echarts)。

<!--more-->

## 关于echarts
echarts是百度推出的，使用JavaScript实现的开源可视化库，可以提供直观、可定制化的数据可视化图表，包括折线图、柱状图、散点图、地图和饼图等，[点击跳转主页](http://echarts.baidu.com/)。

## 使用
### 需求
使用之前先谈需求：使用echarts的话，需求基本上都是为了实现数据可视化，那么数据可视化牵扯到一个怎么展示的问题，就echarts功能而言，展示将通过可视化图表进行，也就是，这里的需求可归为将某数据使用echarts以图表（假定为柱状图）的形式呈现。

### 实现
一般情况，我会直接打开[echart官方实例](http://echarts.baidu.com/examples/index.html), 选取相应的实例，我们以最简单的[折线图](http://echarts.baidu.com/examples/editor.html?c=line-simple)为例。可以看到对应的JS代码为：
```
option = {
    xAxis: {
        type: 'category',
        data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
    },
    yAxis: {
        type: 'value'
    },
    series: [{
        data: [820, 932, 901, 934, 1290, 1330, 1320],
        type: 'line'
    }]
};
```
那么，这里的各种变量分别代表的是什么含义呢？当然可以在此修改部分数据，查看折线图的变化，echarts图形化的呈现是通过setOption配置方法来实现的，[点击详情](http://echarts.baidu.com/option.html#title)，这里对各种配置做出详尽的介绍。

### 简单实例
那么在日常开发环境中如何实现呢？
首先，创建first.html文件，并编写：
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>echarts使用</title>  
    <link rel="stylesheet" href="css/style.css">
    <script src="js/echarts.min.js"></script> 
</head>
<body>
 	<div class="content">
 		<div class="title">
 			echarts使用案例
 		</div>
 		<div class="chart">
 			<div id="firstchart">
	 		</div>
 		</div>
 	</div>

 <script type="text/javascript">
 	var myChart = echarts.init(document.getElementById('firstchart'));
	var option = {
		title: {
            text: '一周温度变化',
            left: 'center',
            top: '1%',
            textStyle: {
                color: 'white',
                fontSize:16,
            }
        },
	    xAxis: {
	        type: 'category',
	        data: ['星期一', '星期二', '星期三', '星期四', '星期五', '星期六', '星期天'],
	        axisTick: {
                alignWithLabel: true
            },
            axisLine:{
                lineStyle: {
                    color:'white',
                }
            },
	    },

	    yAxis: {
	        type: 'value',
	        axisTick: {
                alignWithLabel: true
            },
            axisLine:{
                lineStyle: {
                    color:'white',
                }
            },
	    },
	    series: [{
	        data: [11, 12, 15, 5, 8, 14, 9],
	        type: 'line'
	    }]
	};

	myChart.setOption(option);
 </script>
</body>
</html>
```
编写对应的css样式文件，打开浏览器就可看到对应的折线图。

### 定制
在平时的使用中，其它需求势必存在，而echarts本身也提供了这种定制化配置。下面关于一些常见需求举例说明：
### 添加图注
就上述折线图而言，气温变化一般可分为最高温度变化和最低温度变化，也就意味着会有两条折线。为了更直观表现，这里可使用图注来说明,在配置项中legend属性：
```
legend:{
	right:0,
    top:3,
    textStyle:{
        fontSize:12,
        color:'#FFF',
    },
    data:['最高温度变化']
},
```

并设置了图注属性。

### 将坐标名旋转
有时，由于图标可占用的空间有限，加之，坐标名字符较长，就导致坐标名显示不全的情况，这时候，可以将做表明改为斜体展示（旋转角度），通过在xAxis中添加axisLabel属性来实现：
```
xAxis : [
    {
    axisLabel:{
        interval:0,
        rotate:45,//倾斜度 -90 至 90 默认为0
        },
    }
]
```
### 设置坐标网格背景
为了界面的美观，可以在图表中绘制网格，并设置网格背景。首先，在xAxis下添加：
```
splitLine:{
    show:true,
    lineStyle:{
        color: 'white',
        width:1,
        type: 'solid'
    }
},
```

这样，绘制了网格，然后，在yAxis中添加：
```
splitArea:{
    show:true,
    areaStyle:{
        color:['#6a6f71', '#5b6062'],
    },
},
```

即可实现网格背景。


以上。
本文将持续更新。






