---
title: h5-meta
date: 2017-07-06 15:58:22
tags: [笔记,meta]
---

# meta标签的作用
搜索引擎优化（SEO），定义页面使用语言，自动刷新并指向新的页面，实现页面转换时的动态效果，控制页面缓冲，网页定级评论，控制网页显示的窗口等
<!--more--> 
# meta标签的组成
meta主要有两个属性，分别是http-equiv属性和name属性，

|属性|值|描述|
|:--|:--|:--|
|http-equiv|	content-type<br>expires<br>refresh<br>set-cookie|把 content 属性关联到 HTTP 头部。|
|name|	author<br>description<br>keywords<br>generator<br>revised<br>others|把 content 属性关联到一个名称。|
|scheme|some_text|定义用于翻译 content 属性值的格式。|
<br>

# H5常用meta设置

## viewport
```
<meta name="viewport" 
	content="width=device-width,
			height=device-height,
			initial-scale=1.0, 
			minimum-scale=1.0, 
			maximum-scale=1.0, 
			user-scalable=no">
```
<b>width = [pixel_value | device-width]</b>   
控制viewport的大小，可以指定一个值或者特殊的值，如device-width为设备的宽度（单位为缩放为100%是的CSS的像素）。   


<b>height = [pixel_value | device-height]</b>   
和width相对应，指定高度。

<b>initial-scale = float_value </b>   
初始缩放。即页面初始缩放程度。为浮点值，是页面大小的一个乘数。例如，如果你设置初始缩放为‘1.0’，那么，web页面在展示的时候就会以target density分辨率的1:1来展现。如果设置为‘2.0’，那么这个页面就会放大为2倍。   

<b>maximum-scale = float_value</b>   
最大缩放。即允许的最大缩放程度。浮点值，用以指出页面大小与屏幕大小相比的最大乘数。例如，将值设置为‘2.0’，那么页面与target size相比，最多能放大2倍。

<b>user-scalable = [yes | no]</b>   
用户调整缩放。即用户能否改变页面缩放程度。如果设置为yes，表示允许用户对其改变，反之为no。默认值为yes。如果设置为no，那么mininum-scale和maximum-scale都将被忽略，因为根本不可能缩放。

所有的缩放值都必须在0.01 - 10的范围之内。

安卓中还支持`target-densitydpi`这个私有属性，它表示目标设备的密度等级，作用是觉得css中的1px代表多少物理像素   
<b>target-densitydpi = [high-dpi | medium-dpi | low-dpi | device-dpi]</b>   
当`target-densitydpi=device-dpi`时，css中的1px会等于物理像素中的1px。   
因为这个属性只有安卓支持，并且安卓已经决定要废弃这个属性了，所以这个属性我们要避免使用。

<br>

----

## IOS设备：
## format-detection
用来检测html里的一些格式。主要属性设置有：   

```
<meta name="format-detection" content="telephone=no,email=no,adress=no">
```

<b>telephone:</b>    
使设备浏览网页时对数字不启用电话功能（不同设备结束不同，iTouch点击数字存入为联系人，iPhone手机上为拨号的超链接），忽略将页面中的数字识别为电话号码。`telephone=no`就禁止了把数字转化为拨号链接。需启用电话功能将`telephone=yes`即可，若在页面上面有 Google Maps, iTunes 和 YouTube 的链接会在ios设备上打开相应的程序组件。

<b>email</b>   
告诉设备不识别邮箱，点击之后不自动发送。`email=no`禁止作为邮箱地址。默认开启。

<b>adress</b>   
`adress=no`禁止跳转至地图！默认开启。


## apple-mobile-web-app-capable
是否启用 WebApp 全屏模式，删除默认的苹果工具栏和菜单栏。content有两个值‘yes’和‘no’。当需要显示时meta就不用加了，默认就是显示。   

```
<meta name="apple-mobile-web-app-capable" content="yes">
```

## apple-mobile-web-app-status-bar-style
控制状态栏显示样式（当启动webapp功能时，显示手机信号、时、电池的顶部导航栏的颜色）。默认为default(白色)，可以定为black(黑色)和black-translucent(灰色半透明)。

在iOS中有两个meta值，apple-mobile-web-app-capable和apple-mobile-web-app-status-bar-style，这两个会让网页内容以应用程序风格显示，并使状态栏透明。

## apple-mobile-web-app-title
添加到主屏后的标题（iOS 6 新增）   

```
<meta name="apple-mobile-web-app-title" content="标题">
```
## apple-itunes-app
添加智能 App 广告条 Smart App Banner（ios 6+ Safari）:告诉浏览器这个网站对应的app，并在页面上显示下载banner

```
<meta name="apple-itunes-app" content="app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL"> 
```

<br>

----

## 其他
```
<!-- 针对手持设备优化，主要针对一些老的不识别viewport的浏览器，比如黑莓 -->
<meta name="HandheldFriendly" content="true">

<!-- 微软的老式浏览器 -->
<meta name="MobileOptimized" content="320">

<!-- UC强制竖屏 -->
<meta name="screen-orientation" content="portrait">

<!-- QQ强制竖屏 -->
<meta name="x5-orientation" content="portrait">

<!-- UC强制全屏 -->
<meta name="full-screen" content="yes">

<!-- QQ强制全屏 -->
<meta name="x5-fullscreen" content="true">

<!-- UC应用模式 -->
<meta name="browsermode" content="application">

<!-- QQ应用模式 -->
<meta name="x5-page-mode" content="app">

<!-- windowsphone点击无高光-->
<meta name="msapplication-tap-highlight" content="no">
```
