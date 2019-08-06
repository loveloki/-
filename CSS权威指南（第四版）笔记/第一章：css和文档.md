# 第一章：css和文档

**层叠样式表** (Cascading Style Sheets，缩写为 **CSS**），是一种 [样式表](https://developer.mozilla.org/zh-CN/docs/DOM/stylesheet) 语言，用来描述 [HTML](https://developer.mozilla.org/zh-CN/docs/HTML)或 [XML](https://developer.mozilla.org/zh-CN/docs/XML_介绍)（包括如 [SVG](https://developer.mozilla.org/zh-CN/docs/SVG)、[MathML](https://developer.mozilla.org/zh-CN/docs/Web/MathML)、[XHTML](https://developer.mozilla.org/zh-CN/docs/XHTML) 之类的 XML 分支语言）文档的呈现。CSS 描述了在屏幕、纸质、音频等其它媒体上的元素应该如何被渲染的问题。

## Web样式简介（CSS背景与发展）

Web早期，浏览器为用户提供了各种样式的定制功能，但是文档编写人员却不能制定样式。CSS就是在这样的背景下诞生的（当时已经有过一些样式表语言的建议了，但CSS是第一个含有“层叠”主意的）。

CSS的目标是提供一个简单的声明式样式语言，而且具有一定的灵活性，能够为文档编写人员和用户提供等同的样式化功能。层叠样式表中的**层叠**是指样式可以结合起来使用，而且具有优先级，文档编写人员和用户都有话语权，但是最终决定权在用户手中。

1994年CSS发布第一个草案，1996年年末CSS1完成了。此后CSS工作组开始制定CSS2，而各浏览器则开始实现CSS1。单独来看，CSS每一部分都很简单，但把各部分放在一起就会变得异常复杂。而且早期实现有些先天不足，例如不同浏览器对盒模型（box model）的实现之间有差异尤其为人诟病。

1998年年初CSS2定案。随后CSS工作组开始制定CSS3，以及CSS2的修订工作（制定CSS2.1）。与以往不同的是，CSS3有多个（理论上的）独立的模块构成，而不是单独一个臃肿的规范。

CSS3分成多个模块的根本原因是各模块可以独立演进，尤其是重要的（或是受众广的）模块可以按照W3C的规划向前推进，而不必受其他模块拖累。

事实证明，这样做是对的。

但是这样做也有缺点，那就是**CSS规范不能涵盖一切**。所以我们没有办法指着某些文件说：这就是CSS3，而是**应该分模块学习不同的特性**。

如果要看单独的完整规范，可以留意CSS工作组每年发布的[Snapshot 文档 ](https://www.w3.org/Style/CSS/)。

## 元素

元素（element）是文档结构的根基，文档中每个元素都对文档的表现起一定作用。

### 置换元素和非置换元素

对CSS来说，元素通常有两种形式：**置换元素**和**非置换元素**

**置换元素**（replaced element）指用来置换元素内容的部分不由文档内容直接表示。

常见的置换元素有**img**，**input**等。

**非置换元素**（nonreplaced element）指元素内容由用户代理（通常为浏览器）在元素自身生成的框中显示。

大部分HTML元素都是非置换元素，比如**p**，**span**等。

### 元素的显示方式

除了置换元素和非置换元素，CSS还把元素分为**块级**和**行内**两种基本类型。（这两种最常见，除此之外还有其他的显示类型）。

**块级元素**默认生成一个填满父级元素内容区域的框，旁边不能有其他元素。也就是说，块级元素在元素框前后都有**断行**。常见的块级元素有**p**和**div**。

**列表项目是一种特殊的块级元素**，它会在元素框旁边生成一个记号（无序列表通常是圆点，有序列表通常是数字），除此之外列表项目和其他块级元素并没有什么不同。

**行内元素**在一行文本内生成元素框，不打断所在的行。最常见的行内元素是**a**。

**在HTML中，块级元素不能出现在行内元素中**。但是在CSS中没有这个限制，可以随便嵌套。

## 把CSS应用到HTML上

### link标签

```html
<link rel="stylesheet" type="text/css" href="sheet.css" media="all">
```

**link标签的作用是把其他文档与当前文档关联起来**。

CSS使用link链接的样式表不是HTML文档的一部分，但却供文档使用。我们称这样的样式表为**外部样式表**（external stylesheet）。

为了正确的加载样式表，link标签必须放在head元素中，不能放在其他元素中。

#### 属性

1. rel是relation（关系）的简称，这里指定的是stylesheet。
2. type属性的值为text/css，说明通过link标签加载的数据类型。（可省略不写，默认为text/css）。
3. href的值是样式表的URL。可以是绝对地址，或者相对地址。
4. media属性，它的值是一个或多个媒体描述符（media descriptor），可以省略。

#### 候选样式表

候选样式表（alternate stylesheet）的定义方式是把rel属性的值设为alternate stylesheet。仅当用户自己选择，文档才会使用候选样式表渲染。

如果浏览器支持候选样式表，会使用link元素title属性的值生成候选样式表列表。

```html
<link rel="stylesheet" type="text/css" href="sheet1.css" title="Default">
<link rel="stylesheet" type="text/css" href="sheet2.css" title="Big Text">
<link rel="stylesheet" type="text/css" href="sheet3.css" title="Crazy Colors">
```

浏览器默认使用第一个样式表（这里是名为Default的样式表）。此外用户还可以自己选择想使用的样式表（实际上这就是CSS开始崭露头角之时的方式）。

下图为Firefox提供的候选样式表选项

![1565024877015.png](https://user-gold-cdn.xitu.io/2019/8/6/16c672eef477d59d?w=1123&h=246&f=png&s=34962)

**多数基于Gecko的浏览器，IE11支持候选样式表，基于Webkit的浏览器不支持候选样式表。**

### style 元素

style元素也是一种引入样式表的方式，直接写在文档中：

```html
<style type="text/css">...</style>
```

开始与结束style标签之间的样式表称为文档样式表（document stylesheet）或嵌入样式表（embedded stylesheet）。style元素可以直接包含应用到文档上的样式，也可以通过@import 指令引入外部样式表。

### @import 指令

与link标签一样，Web浏览器遇到@import 指令时会加载外部样式表。但是**@import 指令必须写在样式表的开头**，否则不起作用。

一个文档中**可以有多个**@import 指令，但是**无法指定候选样式表**，**可以指定媒体描述符**。

### 行内样式

如果只是为单个元素提供少量样式，可以利用HTML元素的style属性设置行内样式。

```html
<p style="color: red;">这是一段红色的字</p>
```

除了body元素之外的标签，所有的HTML标签都能设置style属性。

style属性中不能使用@import 指令。

### HTTP链接

为文档关联CSS还有一种鲜为人知的方式：**使用HTTP首部**。

在支持HTTP链接样式表的浏览器中（比如Firefox系列），可以用此方式将开发版与线上部署版区分开。

## 样式表中的内容

### 标记

样式表中除了HTML注释外，不能有任何标记。

### 规则的结构

```css
h1 {
    color: red;
    background: yellow;
}
```

一个样式表由一系列**规则**组成。一个规则由两个基本部分构成：**选择符**（selector）和**声明块**（declaration block）。声明块由一个或多个**声明**组成，而一个声明包含一个**属性**（property）和对应的**值**（value）。

如上面的规则所示：选择符为h1。声明块为大括号内的内容。声明块里面有两个声明：字体颜色为红色，背景颜色为黄色。所以整个规则为：对于文档中的所有h1元素，显示为黄底红字。

### 厂商前缀

CSS有些内容的前面有个标注，例如-o-border-image，这就叫做厂商前缀，浏览器通过它标记实验性或专属的属性、值或其他内容。

下表为一些常见的厂商前缀

| 前缀     | 厂商                                   |
| :------- | :------------------------------------- |
| -epub-   | 国际数字出版论坛制定的ePub格式         |
| -moz-    | 基于Mozilla的浏览器（如Firefox）       |
| -ms-     | 微软IE浏览器                           |
| -o-      | 基于Opera的浏览器                      |
| -webkit- | 基于Webkit的浏览器（如Safari和Chrome） |

厂商前缀的一般格式为一个英文破折号、一个标注和一个英文破折号。不过也有少量前缀开头没有破折号。

### 处理空白

CSS对规则之间的空白基本没有严格要求，而且对规则内部的空白也没有严格要求，不过有些例外。

一般来说，CSS对待空白的方式跟HTML差不多：**解析时，连续的空白会被合并为一个空白**（不论空白是空格，制表符还是换行符，甚至是他们的组合）。

下面这几种编写方式效果一样

```css
p{color:pink;}
p {color: pink;}
p {
    color: pink;}
p {
    color: pink;
}
p
{
    color
    :
        pink
        ;
}
```

### CSS注释

CSS支持注释，注释内容放在 /* 和 */之间，**可以换行**但是**注释不能嵌套**。

正确的注释：

```css
/*这是一个正确的注释，
注释可以换行。*/
```

错误的注释：

```css
/*这是一个错误的注释，
	/*CSS的注释不能嵌套*/
*/
```

## 媒体查询

文档编写人员通过媒体查询（media query）定义浏览器在何种媒体环境中使用指定的样式表。

### 用法

媒体查询可以在下述几个地方使用：

- link元素的media属性
- style属性的media属性
- @import 声明的媒体描述符部分
- @media 声明的媒体描述符部分

媒体查询可以是简单的媒体类型，也可以是复杂的媒体类型和特性的组合。

### 简单的媒体查询

这个例子中，在所以媒体中h1元素都是红褐色，但是在投影媒体中body元素会有一个黄色背景。

```css
h1{
    color: red;
}
@media projection {
    body{
        background: yellow;
    }
}
```

一个样式表中可以有任意多个@media块，而且每个都有自己的一套媒体描述符。

### 媒体类型

媒体查询最基本的形式媒体类型，由CSS2引入。媒体类型就是指明不同媒体的标注：

- all：用于所有展示的媒体。
- print：为有视力的用户打印文档时使用，也在预览打印效果时使用。
- screen：在屏幕媒体（比如电脑的显示器，Web浏览器）上展示文档时使用。

多个媒体类型使用逗号分隔，比如同时应用到屏幕媒体和印刷媒体上：

```css
@media screen, print{
	...
}
```

### 媒体描述符

一个媒体描述符包含一个媒体类型和一个或多个媒体特征列表，其中特征描述符要放在圆括号中。如果没有媒体类型，那就应用到所有媒体上面。

以下两个语句是等效的：

```css
@media all and (min-resolution: 96dpi){...}
@media (min-resolution: 96dpi){...}
```

一般情况下，媒体特性描述符的格式类似于CSS中的一对属性和值。两者间最大的区别是**特性描述符可以不包含值**。因此，任何彩色媒体都符合（color）指定的条件。任何色深为16位的彩色媒体都符合（color：16）指定的条件。其实，不指定值的时候是在做判断。比如说，（color）的意思是：这个媒体是彩色的吗？

媒体查询可用的**逻辑关键字**有两个：

- and：连接的两个或多个媒体特性必须同时满足条件，整个查询结果才为真值。

	```css
	(color) and (orientation: lamdscape) and (min-device-width: 800px)
	/*
	    表示当媒体环境是彩色的，横着放的，而且设备宽度至少是800px像素时，才会应用样式表。
	*/
	```

	

- not：对整个查询取反。只能用在媒体查询开头。

	```css
	(color) and not (min-device-width: 800px)
	/*
	    错误！
	    not只能放在媒体查询开头，这样写会被忽略。
	*/
	```

	

媒体查询**不支持OR关键字**。不过，分隔多个媒体查询的逗号相当于OR。

**此外还有一个only关键字，专门用来保证向后兼容。**

only：在不支持媒体查询的旧浏览器中隐藏样式表。**只能用在媒体查询开头。**

```css
@import url(new.css) only all;
/*
	在支持媒体查询的浏览器中，only会被忽略。
	在不支持媒体查询的浏览器中，媒体类型为only all，无效，这个媒体查询语句会被忽略。
*/
```

### 媒体特性描述符和值的类型

| 至2017年年底所有可用的描述符 |                         |                 |
| ---------------------------- | ----------------------- | --------------- |
| width                        | max-device-height       | min-color-index |
| min-width                    | aspect-ratio            | max-color-index |
| max-width                    | min-aspect-ratio        | monochrome      |
| device-width                 | max-aspect-ratio        | min-monochrome  |
| min-device-width             | device-aspect-ratio     | max-monochrome  |
| max-device-width             | min-device-aspect-ratio | resolution      |
| height                       | max-device-aspect-ratio | min-resolution  |
| min-height                   | color                   | max-resolution  |
| max-height                   | min-color               | orientation     |
| device-height                | max-color               | scan            |
| min-device-height            | color-index             | grid            |

此外，还有两种新增的值：

- ratio
- resolution

## 特性查询

2015~2016年间，CSS新增了一个功能，根据用户代理是否支持特定的CSS属性及其值来应用一段样式。这个功能被称为特性查询。

```css
@supports (color: blank) {
    body {color: blank;}
    h1 {color:pink;}
}
```

上述代码的意思是：如果可以识别并处理**color： blank**这样的属性和值的组合，那么就应用这段样式。否则忽视它。

**特性查询是渐进增强的完美方式。**

与媒体查询一样，特性查询支持使用逻辑运算符**and**，**or**，**not**

特性查询为什么既要写属性也要写值？比如测试是否支持栅格布局：

```css
@supports (display) {
    /* 栅格样式 */
}
```

这样写肯定不行，因为支持display属性的不一定支持grid这个值。

**注意：特性查询不是正确性查询，无法保证所有支持的浏览器的实现效果一样。**