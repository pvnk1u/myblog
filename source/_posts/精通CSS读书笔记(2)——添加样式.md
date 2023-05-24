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

