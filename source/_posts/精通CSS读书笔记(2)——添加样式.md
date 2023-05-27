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



一些最常见的用于超链接的伪类列举如下。在涵盖最常见HTML元素的基础样式表中，应该始终包括它们：

```css
/* 未访问过的链接为蓝色 */ 
a:link{
	color: blue;
}

/* 访问过的链接为绿色 */
a:visted{
	color: green;
}

/* 超链接在鼠标悬停及获取键盘焦点时为红色 */
a:hover,
a:focus{
	color: red;
}

/* 活动状态时为紫色 */
a:active{
	color: purple
}
```

以上伪类的先后次序很重要。:link和:visited应该排在前面，然后才是与用户交互相关的那些。这样一来，当用户鼠标悬停在链接上，或者链接获得键盘焦点时，:hover和:focus规则会覆盖:link和:hover规则。最后，当鼠标点击或键盘回车选择链接时，应用:active规则。



### 目标与反选

另一个有用的伪类是:target，它匹配的元素有一个ID属性，而且该属性的值出现在当前页面URL末尾的井号（#）后边。如果打开链接http:example.com/blog/1/#comment-3，找到该页面中标记为`<article class="comment" id="comment-3">...</article> `的评论，那么可以通过以下规则高亮该条评论：

```css
.comment:target{
	background-color: #fffec4;
}
```

现在，假设想高亮一条评论，而该评论不是因投票否决而被隐藏的。也有一个选择符专门用于排除某些选择符：它就是反选（negation）伪类，或者:not()选择符。如果被标记为“投票否决”(downvoted)的评论都有一个特殊的类名，那么就可以像下面这样来改写规则：

```css
.comment:target:not(.comment-downvoted){
	backgroud-color: #fffec4;
}
```



##  结构化伪类

CSS3新增了一大批与文档结构相关的新伪类。其中最常用的是nth-child选择符，可以用来交替地为表格应用样式：

```css
tr:nth-child(odd){
	background: yellow;
}
```

这条规则会从表格的第一行开始，将后面每隔一行的背景变成黄色。nth-child选择符就像一个函数，可以接受很多不同的表达式作为参数。如前例所示，它可以接受odd（奇数）和even（偶数）作为参数。这个参数还可以直接是数值，表示目标元素的序数位置。



还有一个伪类选择符也支持这种表达式，比如：

```css
:nth-last-child(N)
```

:nth-last-child选择符与:nth-child选择符类似，只不过是从最后一个元素倒序计算。



## 表单伪类

HTML5为表单输入框新增了几个属性，表示必填的required就是其中之一。

```html
<label for="field-name">Name: </label>
<input type="text" name="field-name" id="field-name" />
```

如果想高亮这个必填控件，可使用:required伪类来选择带有required属性的表单元素，并给它的边框设置一个不同的颜色。

```css
input:required{
	outline: 2px solid #000;
}
```

类似的，可以像下面这样使用：optional伪类，为没有required属性的控件添加样式：

```css
input:optional{
	border-color:#ccc;
}
```

此外，还有针对有效和无效控件的伪类。为满足某个输入框要求填写类型内容（如电子邮件地址）的需求，HTML5也为type属性新增了不少输入值，比如email：

```html
<input type="email" />
```

然后可以根据输入框中当前内容的有效性，应用不同的样式：

```css
/* 如果输入框中包含有效的电子邮件地址 */
input[type="email"]:valid{
	border-color: green;
}

/* 如果输入框中的内容不是有效的电子邮件地址 */
input[type="email"]:invalid{
	border-color: red;
}
```

除此之外，还有针对type值为number的:inrange、:out-of-range伪类，针对readonly属性的:read-only伪类，以及针对没有readonly属性的:read-write伪类。



# 层叠

稍微复杂点的样式表中都可能存在两条甚至多条规则同时选择一个元素的情况。CSS通过一种叫作层叠（cascade）的机制来处理这种冲突。从CSS这个名字就可知这种机制有多重要，因为其中的C就是cascade。层叠机制的原理是为规则赋予不同的重要程度。最重要的是作者样式表，即由网页开发者所写的样式。其次是用户样式表，用户可以通过浏览器的设置选项，为网页应用自己的样式。排在最后的是浏览器（或用户代码）的默认样式表，它们一般都会被作者样式表覆盖掉。为了给用户更高的优先权，CSS允许用户使用!import覆盖任何规则，包括网站作者使用!important标注的规则。!important标注要放在属性声明的后面：

```css
p{
	font-size: 1.5em !important;
	color: #666 !important;
}
```

 允许用户使用!important标注来覆盖规则，主要是出于无障碍交互的需要。比如，允许诵读困难的用户使用高对比度的用户样式表。



归纳起来，层叠机制的重要性级别从高到低如下所示：

1. 标注为!important的用户样式
2. 标注为!important的作者样式
3. 作者样式
4. 用户样式
5. 浏览器（或用户代码）的默认样式

在此基础上，规则再按选择符的特殊性排序。特殊性高的选择符会覆盖特殊性低的选择符。如果两条规则的特殊性相等，则后定义的优先。





# 特殊性

为了量化规则的特殊性，每种选择符都对应着一个数值。这样，一条规则的特殊性就表示为其每个选择符的累加值。但这里的累加计算使用的并非正常的十进制加法，而是基于位置累加，以保证10个类选择符（或者40个，甚至更多的类选择符）累加的特殊性不会大于等于1个ID选择符的特殊性。这是为了避免ID这种高特殊性选择符被一堆低特殊性选择符（如类型选择符）的累加值所覆盖。如果某条规则中用到的选择符不足10个，为简单起见，也可以使用十进制来计算其特殊性。



任何选择符的特殊性都对应于如下4个级别，即a、b、c、d：

1. 行内样式，a为1
2. b等于ID选择符的数目
3. c等于类（class）选择符、伪类选择符及属性选择符的数目
4. d等于类型（type）选择符和伪元素选择符的数目



根据以上规则，可以计算出CSS选择符的特殊性。下表是一些选择符以及它们所对应的特殊性规则：

| 选择符                  | 特殊性  | 十进制特殊性 |
| ----------------------- | ------- | ------------ |
| style=""                | 1,0,0,0 | 1000         |
| #wrapper #content{}     | 0,2,0,0 | 200          |
| #content .datePosted{}  | 0,1,1,0 | 110          |
| div#content{}           | 0,1,0,1 | 101          |
| #content{}              | 0,1,0,0 | 100          |
| p.comment .datePosted{} | 0,0,2,1 | 21           |
| p.comment{}             | 0,0,1,1 | 11           |
| div p{}                 | 0,0,0,2 | 2            |
| p{}                     | 0,0,0,1 | 1            |

乍一看这种计算特殊性的方式有点不好理解，再多解释一下。本质上而言，如果样式被写在了元素的style属性里，那么这些样式的特殊性就最高。然后，通过ID属性应用的规则，其特殊性高于未通过ID属性应用的规则。同理，通过类选择符应用的规则，其特殊性高于只通过类型选择符应用的规则。最后，如果两条规则拥有相等的特殊性，则优先应用后定义的规则，也就是层叠机制。



## 利用层叠次序

如果两条规则特殊性相等，则优先应用后定义的规则，这一点非常重要。这意味着我们在写样式的时候，必须考虑规则在样式中的位置，以及选择符的次序。



前面的对链接元素使用伪类的例子，就是一个利用层叠次序的典型。如果每个选择符的特殊性都一样，那么它们的次序就很重要了。要是把a:visited选择符放在a:hover选择符后面，那么在访问过链接之后，悬停样式将不会起作用，因为已经被a:visted样式给覆盖了。





## 控制特殊性

理解特殊性是写好CSS的关键，而控制特殊性则是大型网站开发中最难处理的问题。利用特殊性，可以先为公用元素设置默认样式，然后在更特殊的元素上覆盖这些样式。在下面的例子中，为介绍性内容定义了几种不同的样式。首先将介绍性文本的颜色设为灰色，覆盖body元素上定义的默认黑色。而在主页上，介绍性文本的样式变成了浅灰色背景上的黑色字体，其中的链接是绿色：

```css
body{
	color: black;
}

.intro{
	padding: 1em;
	font-size: 1.2em;
	color: gray;
}

#home .intro{
	color: black;
	background: lightgray;
}

#home .intro a{
	color: green;
}
```

以上几条规则包含了太多的特殊性。对于小网站这不是问题，但随着网站越来越大，样式也越来越复杂，这样定义规则会导致样式难以管理。这是因为，要想给主页中的介绍性文本添加样式，规则中必须至少包含一个ID选择符和一个类选择符。



比如，假设一个组件中包含着类为call-to-action的链接，为了让这个链接看上去更像按钮，可以通过如下规则为它应用背景颜色和内边距：

```css
a.call-to-action{
	text-decoration: none;
	background-color: green;
	color: white;
	padding: 0.25em;
}
```

把这个call-to-action链接放到主页的介绍性内容中，会出现什么效果？会不怎么好看，因为链接上的文本不见了：由于给定链接样式（#home.intro a）的特殊性高于这个组件样式（a.call-to-action）的特殊性，绿色背景上的文本也成了绿色的。



怎么办？必须想办法提高特殊性，比如给call-to-action组件加上更厉害的选择符：

```css
a.call-to-action,
#home .intro a.call-to-action{
	text-decoration: none;
	background-color: green;
	color: white;
	padding: 10px;
}
```

然而像这样因样式表增大而被迫提高特殊性，会导致选择符之间特殊性的竞争，最终导致代码不必要地复杂化。



更好的做法是从一开始就简化选择符、降低特殊性：

```css
body{
	color: black;
}

.intro{
	font-size: 1.2em;
	color: gray;
}

.intro-highlighted{
	color: black;
	background: lightgray;
}

.intro-highlighted a{
	color: green;
}

a.call-to-action{
	text-decoration: none;
	background-color: green;
	color: white;
	padding: 10px;
}
```

以上重写的代码改进了两个方面。首先，去掉了ID选择符，把所有选择符的特殊性降到最低。其次，去掉了对介绍性文本上下文的引用。不再将介绍性文本限定为必须在主页中，而只在原始介绍性文本基础上再命名一个特殊的版本（即intro-highlighted）。于是在标记中可以这样使用类：

```html
<p class="intro">A general intro</p>
<p class="intro intro-highlighted">....<a href="/promo-page" class="call-to-action">promo page</a>.</p>
```

这种简化的、目标更明确的手段让作者可以对样式进行更细粒度地控制。intro-highlighted链接的样式不会再覆盖到call-to-action链接的颜色。与此同时，无须修改CSS，即可将intro-highlighted重用到其他页面，这又是一个好处。



## 特殊性与调试

特殊性对于调试而言非常重要，因为需要知道哪条规则优先，以及为什么优先。比如，假设有以下规则：

