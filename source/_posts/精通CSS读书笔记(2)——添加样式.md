---
title: "精通CSS读书笔记(2)——添加样式" 
date: 2023-05-24T22:34:39+08:00
draft: false
tags: ["web","css"]
categories:
  - "web"
  - "css"
toc: true
---



# CSS选择符

类型与后代选择符是最基本的选择符。类型选择符用于选择特定类型的元素，比如段落或标题元素，只要写出想添加样式的元素名即可。类型选择符有时候也被称为元素选择符。



```css
p{
	color: black;
}
```

后代选择符用于选择某个或某组元素的后代。后代选择符的写法是在两个选择符之间添加空格。在下面的例子中，只有作为块引用后代的段落元素会被选中，从而缩进，其他段落都不会缩进：

```css
blockquote p{
	padding-left: 2em;
}
```

类型选择符与后代选择符非常适合全面应用基础样式。要想更精确地选择目标元素，可以使用ID选择符和类选择符。这两个选择符通过对应ID和class属性的值来选择元素。ID选择符由j井号(#)开头，类选择符由句点（.）开头。下面例子中的第一条规则会把介绍性段落中的文字变成粗体，而第二条规则会把日期变成灰色。

```html
#intro{
	font-weight: bold;
}

.date-posted{
    color: #ccc;
}

<p id="intro">
    Happy Birthday
</p>

<p class="date-posted">
    20/1//2013
</p>
```



有时候，可以将ID和类选择符与类型和后代选择符组合起来使用，而不必为所有元素都添加ID和类选择符：

```html
#latest h1{
	font-size: 1.8em;
}

#latest .date-posted{
	font-weight: bold;
}

<article id="latest">
    <h1>Happy Birthday</h1>
    <p class="date-posted"><time datetime="2013-01-20">20/1/2013</time></p>
</article>
```



## 子选择符与同辈选择符

除了基本选择符，CSS也提供了高级选择符。第一个高级选择符叫子选择符。与后代选择符会选择一个元素的所有后代不同，子选择符只选择一个元素的直接后代，也就是子元素。在下面的例子中，外部列表中的列表项前面会出现自定义的图标，而嵌套列表中的列表项则不会受影响：

```html
#nav > li{
	background: url(folder.png) no-repeat left top;
	padding-left: 20px;
}

<ul id="nav">
	<li><a href="/home/">Home</a></li>
	<li><a href="/services/">Services</a>
		<ul>
			<li><a href="/services/design/">Design</a></li>
			<li><a href="/services/development/">Development</a></li>
			<li><a href="/services/consultancy/">Consultancy</a></li>
		</ul>
	</li>
	<li><a href="/contact/">Contact Us</a></li>
</ul>
```

有时候可能需要为与某个元素相邻的元素添加样式。使用相邻同辈选择符，就可以选择位于某个元素后面，并与该元素拥有共同父元素的元素。使用相邻后辈选择符，可以为第一个段落中的文本应用粗体、灰色，让它们比后面段落中的文本稍微大一点：

```css
h2 + p{
	font-size: 1.4em;
	font-weight: bold;
	color: #777;
}
```



这样选择标题后面的第一个段落是可行的，但更简单、更容易维护的方式，还是为开头这一段增加一个类名，比如intro-text。这样，intro-text类也可以应用于其他并非直接位于h2元素之后的段落。



`>`和`+`在这里被称为组合子，因为它们描述了自身两侧的选择符组合的方式。实际上还有一个类似的组合子，那就是一般同辈组合子：~。仍以前面的例子来说明，使用一般同辈组合子可以选择h2元素后面的所有段落。

```css
h2 ~ p{
	font-size: 1.4em;
	font-weight: bold;
	color: #777;
}
```



## 通用选择符

通用选择符可以匹配任何元素。与其他语言中的通配符类似，通用选择符也使用星号（*）表示。也就是说，只用一个星号，就可以匹配页面中的所有元素。那么，是否可以使用通用选择符来删除所有元素默认的内外边距呢？比如这样：

```css
*{
	padding: 0;
	margin: 0;
}
```

事实上，这样写可能带来很多意想不到的后果，特别是会影响button、select等表单元素。如果想重设样式，最近还是像下面这样明确指定元素：

```css
h1,h2,h3,h4,h5,h6,
ul,ol,li,dl,p{
	padding: 0;
	margin: 0;
}
```

当然，通用选择符不仅限于给文档中的所有元素设置属性。还可以把它与组合子结合使用，选择某个特定的嵌套层次，此时重要的是层次而不是元素类型。看下面这个例子：

```css
.product-section > *{
	/* ... */
}
```

这个组合选择符会选择带有类名product-section的元素的直接后代，不管它是什么元素，有什么属性。



## 属性选择符

属性选择符是基于元素是否有某个属性或者属性是否有某个值来选择元素。有了这种选择符，可以实现很多更有意思、更深入的选择。



比如，鼠标指针悬停在某个带有title属性的元素上时，多数浏览器都会显示一个提示条。利用这种行为，可以借助`<abbr>`元素对某些缩写词给出详尽的解释：

```html
<p>The term<abbr title="self=contained underwater breathing apparatus">SCUBA</abbr> is an...</p>
```

可是，如果不把鼠标放在这个元素上，谁也不知道它还会显示缩写词的解释。为此，可以使用属性选择符给带有title熟悉的abbr元素添加不同的样式，比如，在缩写词下面加一条点划线。然后把悬停状态的鼠标指针改成问号。

```css
abbr[title]{
	border-botton: 1px dotted #999;
}

abbr[title]:hover{
	cursor: help;
}
```

除了可以根据是否存在某个属性来选择元素，还可以根据特定的属性值来应用样式。比如，下面这个例子可以用来修正一个问题，即鼠标悬停在提交按钮上时，不同浏览器显示的光标不一致。有了这条规则，所有type属性值为submit的input元素在鼠标指针悬停时，都会显示一个手状光标：

```css
input[type="submit"]{
	cursor: pointer;
}
```

有时候更关心的是属性值是否匹配某个模式，而非某个特定值。这时候，通过给属性选择符中的等号前面加上特殊字符，就可以表达出想要匹配的值的形式了。

要匹配以某些字符开头的属性值，在等号前面加上插入符（^）:

```css
a[href^="http:"]
```

要匹配以某些字符结尾的属性值，在等号前面加上美元符号（$）：

```css
ims[src$=".jpg"]
```

要匹配包含某些字符的属性值，在等号前面加上星号（*）：

```css
 a[href*="/about/"]
```

要匹配以空格分隔的字符串中的属性值（比如rel属性的值），在等号前面加上波浪号（~）：

```css
a[rel~=next]
```

还有一个属性选择符，可以选择开头是指定值或指定值后连着一个短划线的情况。要匹配这种情况，在等号后加上竖线（|）：

```css
a[lang|=en]
```



## 伪元素

有时候想选择的页面区域不是通过元素来表示的，而且也不想为此给页面增加额外的标记。css为这种情况提供了一种特殊选择符，叫做伪元素。



首先，可以使用::first-letter伪元素来选择一段文本的第一个字符。若要选择一段文本的第一行，可以使用::first-line。



此外，还有伪元素对应着内容开头和末尾处假想的元素，分别是::before和::after。这两个伪元素非常适合用来插入小图标及版面装饰符号。如果没有它们，要实现同样的视觉效果，就必须在HTML中插入真实的元素。



伪元素结合使用的例子：

HTML:

```html
<h1>A Study In Scarlet</h1>
<section class="chapter">
	<p>In the 1878 I took my degree od Doctor of Medicine of the University of London,and proceeded to Netley to go through the course prescribed for surgeons in the army. Having complted my studies there, I was duly attached to the Fifth Northumberland Fusiliers as Assistant Surgeon.</p>
</section>
```

CSS:

```css
.chapter::before{
	content: '""';
	font-size: 15em;
}

.chapter p::first-letter{
    float: left;
    font-size: 3em;
    font-family: Georaia, Times, "Times New Roman",serief;
}

.chapter p::first-line{
    font-family: Georaia, Times, "Times New Roman",serief;
    text-transform: uppercase;
}
```

这里使用了::first-letter伪元素实现了段落的首字下沉效果。第一行也通过::first-line伪元素转换为全部大写，并应用了不一样的字体。.chapter开头那个装饰性的大引号则利用了::before伪元素。实现这么多视觉效果，却没有额外增加一个元素，确实方便。



## 伪类

有时候会想基于文档结构以外的情形来为页面添加样式，比如基于超链接或表单元素的状态。这时候就可以使用伪类选择符。伪类选择符的语法是以一个冒号开头，用于选择元素的特定状态或关系。



