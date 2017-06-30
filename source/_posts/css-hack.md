---
title: css-hack
date: 2016-12-13 12:50:02
tags: [前端,CssHack]
---

# CSS Hack
这段几天稍微看了一下css hack，就做了个笔记总结一下，主要针对IE6~IE11。但对于hack，我自己不太推荐使用。

## 条件注释法
IE浏览器专有的Hack方式:
`<!--[if IE]> 只在IE下生效 <![endif]-->`

`<!--[if IE 8]> 只在IE8下生效 <![endif]-->`

`<!--[if gte IE 8]> 只在IE8以上(包括IE8)版本IE浏览器显示生效 <![endif]-->`

`<!--[if lt IE 8]> 只在IE8以下(包括IE8)版本IE浏览器显示生效 <![endif]-->`

`<!--[if !IE 8]> 只在IE8上不生效 <![endif]-->`

`<!--[if !IE]><!--> 非IE浏览器生效 <!--<![endif]-->`

但当IE的版本到到达10/11后，开始不支持这种条件性注释

<!--more-->
## 符号前缀
### CSS属性前缀:

`'_'` : IE6
```css
Selector { _property: value; }
```
`'*'` : IE6/7
```css
Selector {*property: value;}
```
`'#'` : IE6/7
```css
Selector {#property: value;}
```

`'+'` : IE7
```css
Selector +property: value;
```
`'\0'` : IE8+
```css
Selector {property: value\0;}
```
`'\9'` : IE6 ~ IE10
```css
Selector {property: value\9;}
```
`'\0\9'` : IE8+
```css
Selector {property: value\0\9;}
```

`'\9\0'` :IE9/10
```css
Selector {property: value\9\0;} 
```
注意hack的书写顺序
### 选择器前缀:
`'*html selector'` : IE6
```css
*html Selector {property: value;}
```
`'*+html selector'` && `'*:first-child+html'` : 针对IE7
```css
*+html Selector {property: value;}
'*:first-child+html' Selector {property: value;}
```

`:root Selector {property: value\9;}`    IE9/10


### @media方法

```css
IE6/7 :
@media screen\9{
	Selector {property: value;}
}
```
```css
IE8 :
@media \0screen{
	Selector {property: value;}
} 
```
```css
IE8- :
@media \0screen\,screen\9{
	Selector {property: value;}
} 
```
```css
IE8+ :
@media screen\0{
	Selector {property: value;}
} 
```
```css
IE9+ :
@media screen and (min-width:0\0){
	Selector {property: value;}
}
```
```css
IE10+ :
@media screen and (-ms-high-contrast: active), (-ms-high-contrast: none){
	Selector {property: value;}
}
```

### Firefox
```css
@-moz-document url-prefix() { 
  .selector { property: value; } 
}
```

### Webkit内核浏览器(chrome and safari)
```css
@media screen and (-webkit-min-device-pixel-ratio:0){
	.selector { property: value; }
}
```
<br/><br/>
### IE10 css hack
#### 特性检测：@cc_on
我们可以利用IE私有的`条件编译`结合条件注释(确保IE6~9不承认它)来提供针对IE10的css hack，然后使用它的特点检测`@cc_on`。

```js 
<!--[if ! IE]><!-->
	<script>
		if(/*@cc_on!@*/false){
			document.documentElement.className+=' ie10';
		}
	</script>
<!--<![endif]-->
```
注意`/*@cc_on!@*/`中间的这个`!`感叹号。   
接下来就可以在ie10中给html元素添加一个class="ie10"，然后针对ie10样式就可以使用这个选择器了，如下：

```css
.ie10 .example {
	/* IE10-only styles go here */
}
```
IE11已不支持`@cc_on`，所以我们也可以这样区分：

```html
<!--[if !IE]><!-->
    <script>
        // 针对IE10
        if (/*@cc_on!@*/false) {
            document.documentElement.className += ' ie' + document.documentMode;
        }
        // 针对IE11及非IE浏览器，
        // 因为IE11下document.documentMode为11，所以html标签上会加ie11样式类；
        // 而非IE浏览器的document.documentMode为undefined，所以html标签上会加ieundefined样式类。
        if (/*@cc_on!@*/true) {
            document.documentElement.className += ' ie' + document.documentMode;
        }
    </script>
<!--<![endif]-->
   

<style type="text/css">
    .ie10 .testclass {
        color: red
    }
    .ie11 .testclass {
        color: blue
    }
    .ieundefined  .testclass {
        color: green
    }
</style>
```
document.documentMode可以查看IEdocumentMode属性（IE8+新增）
