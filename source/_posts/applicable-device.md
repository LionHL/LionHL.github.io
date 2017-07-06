---
title: applicable-device
date: 2017-07-06 16:28:04
tags: [笔记,meta]
---

自适应网站中我们用meta告诉浏览器网页自适应规则：   

```
<meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=yes" />
```
<!--more-->
通用搜索引擎对自适应识别校验代码  
 
```
// no-siteapp和no-transform，是告诉搜索引擎不要把网页转码
<meta http-equiv="Cache-Control" content="no-transform" /> 
<meta http-equiv="Cache-Control" content="no-siteapp" />
```
   
以上的代码已经可以让百度识别自适应网站了，但为了对百度更友好，让它更方便识别，我们可以添加`applicable-device`:   

```
<meta name="applicable-device" content="pc,mobile">
```

这个meta标签，表示页面同时适合在移动设备和PC上进行浏览
