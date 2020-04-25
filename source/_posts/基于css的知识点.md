---
uuid: 41acf6f7-6025-3fbd-584c-180f8e37df20
title: 基于css的知识点
date: 2020-04-25 12:15:56
tags: css
category: css
---
vertical-align:middle这个属性是基于一个元素进行对其
第一种用法，“在表单元格中，这个属性会设置单元格框中的单元格内容的对齐方式。”这很容易理解，如果给一个表格的td加一个vertical-align:middle的样式，表格里面的内容会垂直居中，同样的如果给一个vertical-align:bottom就会底部对齐，如果给一个vertical-align:top就会顶部对齐。
第二种用法，该属性定义行内元素的基线相对于该元素所在行的基线的垂直对齐。假设有两个行内元素a和b，a和b都是div，当a加了一个vertical-align:middle样式之后，b的底部（基线）就会对齐a的中间位置