---
title: 使用pdfkit配置模板并生成文件
description: 
categories: technology
tags: JS
---

最近处理一个需求，用户可在自定义配置发票合同等单据模板并填充数据生成pdf文件，要求包括文本、表格和图片;尝试了很多pdf处理的JS库，大概记录一下踩坑历程。

初期调研尝试了jsPDF，pdfkit和pdfmake这几个库，都支持前后端处理，都支持插入图片。

1.pdfmake

简单对比下来，仅针对处理表格而言，实现起来最容易的库是pdfmake它将栅格化发挥到了极致（基于pdfkit），书写非常简单，但存在一个致命的缺陷是不能处理层叠的问题，而需求里的单据模板要求图片签章能堆叠在文字或表格上，对比之后放弃pdfmake，其它两个库，简单对比一下实现都差不多，但jsPDF的starts比pdfkit多好多，用的人多了碰到问题解决方案应该也多才对,秉着这个想法，先选择了jsPDF.

2.jsPDF

jsPDF的api很容易上手了，于是基于api设计模板配置的原型