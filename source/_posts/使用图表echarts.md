---
uuid: 02530f51-32d5-d64a-97b3-64a5d32f6ecb
title: 使用图表echarts
date: 2020-04-17 21:03:26
tags: vue echarts 图表
category: vue
---
如何在vue中使用图表，是图表不是简单的图标
首先导入使用webpack，node安装
<!-- more -->
+ 通过 npm 获取 echarts，npm install echarts --save，详见“在 webpack 中使用 echarts”
+ 通过 vue 获取 vue add echarts，查询echarts的github
绘制一个简单的图表
引入图表
import echarts from 'echarts'

参考：下面使用的id，vue可使用ref进行获取元素，然后$refs.XXX,之后初始化XXX.setOption({//内容})
```
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>ECharts</title>
    <!-- 引入 echarts.js -->
    <script src="echarts.min.js"></script>
</head>
<body>
    <!-- 为ECharts准备一个具备大小（宽高）的Dom -->
    <div id="main" style="width: 600px;height:400px;"></div>
    <script type="text/javascript">
        // 基于准备好的dom，初始化echarts实例
        var myChart = echarts.init(document.getElementById('main'));

        // 指定图表的配置项和数据
        var option = {
            title: {
                text: 'ECharts 入门示例'
            },
            tooltip: {},
            legend: {
                data:['销量']
            },
            xAxis: {
                data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
            },
            yAxis: {},
            series: [{
                name: '销量',
                type: 'bar',
                data: [5, 20, 36, 10, 10, 20]
            }]
        };

        // 使用刚指定的配置项和数据显示图表。
        myChart.setOption(option);
    </script>
</body>
</html>
```
