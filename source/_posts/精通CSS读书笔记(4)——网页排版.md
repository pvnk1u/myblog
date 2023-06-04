---
title: "精通CSS读书笔记(4)——网页排版" 
date: 2023-06-03T17:28:39+08:00
draft: false
tags: ["web","css"]
categories:
  - "web"
  - "css"
toc: true
---









# CSS的基本排版技术

拿到一个页面时，几乎所有设计师都会先考虑从基本的版式着手。从body元素开始，逐步细化，从而让整个页面具有基本的可行性、层次性和配色。接下来的例子就以此为目标：实现一个示例页面的基本版式。



下面的代码展示了一个非常简单的HTML文档，没有添加任何样式。此时的网页内容并没有乱作一团，这是因为浏览器有默认样式表，它为网页应用了一些必要的排版规则。



```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>A Simple Document About the Moon</title>
</head>
<body>
  <article>
    <h1>The Moon</h1>
    <p>The <strong>Moon</strong> (in Greek: σελήνη <i>Selene</i>, in Latin: <i>Luna</i>) is Earth’s only natural satellite. It is one of the largest natural satellites in the Solar System, and, among planetary satellites, the largest relative to the size of the planet it orbits (its primary). It is the second-densest satellite among those whose densities are known (after Jupiter's satellite Io).</p>

    <p>The Moon is thought to have formed approximately 4.5 billion years ago, not long after Earth. There are several hypotheses for its origin; the most widely accepted explanation is that the Moon formed from the debris left over after a giant impact between Earth and a Mars-sized body called Theia.</p>

    <h2>Orbit</h2>

    <p>The Moon is in synchronous rotation with Earth, always showing the same face with its near side marked by dark volcanic maria that fill between the bright ancient crustal highlands and the prominent impact craters. It is the second-brightest regularly visible celestial object in Earth’s sky after the Sun, as measured by illuminance on Earth’s surface.
    </p>
    <p>Although it can appear a very bright white, its surface is actually dark, with a reflectance just slightly higher than that of worn asphalt. Its prominence in the sky and its regular cycle of phases have, since ancient times, made the Moon an important cultural influence on language, calendars, art, and mythology.</p>

    <h3>Gravitational pull &amp; distance</h3>

    <p>The Moon’s gravitational influence produces the ocean tides, body tides, and the slight lengthening of the day. The Moon’s current orbital distance is about thirty times the diameter of Earth, causing it to have an apparent size in the sky almost the same as that of the Sun, with the result that the Moon covers the Sun nearly precisely in total solar eclipse. This matching of apparent visual size will not continue in the far future. The Moon’s linear distance from Earth is currently increasing at a rate of 3.82 ± 0.07 centimetres per year, but this rate is not constant.</p>

    <h2>Lunar travels</h2>

    <p>The Soviet Union’s Luna programme was the first to reach the Moon with unmanned spacecraft in 1959; the United States’ <abbr title="National Aeronautics and Space Administration">NASA</abbr> Apollo program achieved the only manned missions to date, beginning with the first manned lunar orbiting mission by Apollo 8 in 1968, and six manned lunar landings between 1969 and 1972, with the first being Apollo 11. These missions returned over 380 <abbr title="kilograms">kg</abbr> of lunar rocks, which have been used to develop a geological understanding of the Moon's origin, the formation of its internal structure, and its subsequent history. After the Apollo 17 mission in 1972, the Moon has been visited only by unmanned spacecraft.</p>

    <p class="source">Text fetched from <cite><a href="https://en.wikipedia.org/wiki/Moon">“Moon” article on Wikipedia</a></cite> on <time datetime="2016-02-23">the 23<sup>rd</sup> of February&nbsp;2016.</time></p>
  </article>
</body>
</html>

```



这个简单的文档包含几个标题和一些段落（段落文本中有一些行内元素，以便必要时添加样式），它们都包含在一个article元素中。这个没添加样式的网页虽然勉强能看，但效果非常不理想。我们的目标是以相对较少的样式来提升页面的易读性，并且让它变得更美观。



下面逐条分析要添加的规则，同时介绍相关术语，设置规则的原因，以及所涉及的排版属性背后的原理。



## 文本颜色

默认情况下，浏览器会把绝大多数文本渲染为黑色，白底黑字的对比度极高。足够高的对比度是确保网页阅读无障碍的关键，但也往往会被过度强调。事实上，由于屏幕的高对比度，白底黑字会让大段文本显得过分密集，反而影响可读性。



对于上面的例子而言，标题仍然保持黑色，但正文要改成深蓝灰色。链接还是蓝色，但需要把“活力值”下调一点。

```css
p{
	color: #3b4348;
}

a{
	color: #235ea7;
}
```

## 字体族

字体族（font-family）属性的值是一个备选字体的列表，按优先级从左到右排列：

```css
body{
	font-family: 'Georgia Pro',Georgia,Times,'Time New Roman',serif;
}

h1,h2,h3,h4,h5,h6{
	font-family: Avenir Next,SegoeUI,arial,sans-serif;
}
```

这段CSS分别指定了body元素（以及几乎其他所有元素）和标题（h1、h2....）依次使用的字体，如果前面的字体在对应的操作系统中不存在的话，则依次尝试使用后面的字体。



## 字体大小与行高

几乎所有浏览器中font-size的默认大小都是16像素，除非用户修改过偏好设置。这里不修改默认的font-size，而是选择使用em单位调整特定元素的大小：

```css
h3{
	font-size: 1.314em;
}
```

em单位用于font-size属性时，实际上是一个相应元素继承的font-size缩放因子。比如这里的h3元素，字型大小就是1.314*16=21px。虽然可以直接设置21px，但em更灵活一些。多数浏览器都允许用户缩放整个页面，即使像素单位也可以缩放。而使用em之后，如果用户修改偏好中的默认font-size大小，那么相应元素的大小也会调整。



## 行间距、对齐和行盒子的构造

随着给文本施加更多的控制，各种排版概念之间的关系便浮出水面。为了掌握这些概念，有必要深入理解CSS行内格式化模型，同时多理解一些排版术语。下图展示了一行文本的各个部分，这里仅以示例中第一段开头的两个词为例：

```html
<p>The <strong>Moon</strong>...[etc]</p>
```

![LineFormattedModel](https://pvnk1u.github.io/images/LineFormattedModel.PNG)



每行文本都会生成一个行盒子。行盒子还可以进一步拆分成表示行内元素（比如上面例子中的strong元素）的行内盒子，或者连接两个行内元素的匿名行内盒子。



行内盒子中的内容区显示文本。内容区的高度由font-size的测量尺度，即图中的"Moon"末尾那个1em见方的块，以及这个块与字形本身的关系来决定。



行高指的是行盒子的总高度。更通俗的叫法是行间距，排版术语叫铅空，就是排字员用来分隔字符行的铅块。但与传统排版不同，CSS中的“铅空”始终都会同时应用到行盒子的上方和下方。



计算方法如下：行盒子的整体行高减去font-size，得到的值再平分成两份，也就是半铅空。如果line-height是30像素，而font-size是21像素，那么半铅空就是4.5像素。



1. 设置行高

   设置行高时，需要考虑当前字体大小。在这个例子中，对于Georgia这个font-family，给body设置的line-height值为1.5：

   ```css
   body{
   	font-family: Georgia,Times,'Times New Roman',serif;
   	line-height: 1.5;
   }
   ```

   关键在于行与行之间既不能太密，也不能太疏。对于较大的字体，行间距应该稍大一点。文本的长度和font-size也要考虑，短文本一般设置较小的line-height。

   这里给line-height设置了没有单位的值1.5，意思就是当前font-size的1.5倍。body的font-size为16px，那么默认的line-height就是24px；

2. 垂直对齐

   除了line-height，行内盒子也会受到vertical-align属性的影响。它的默认值是baseline，即子元素的基线与父元素的基线对齐。在示例文章的末尾，有一个日期，其中的序数后缀“rd”包含在一个span中：

   ```html
   <time datetime="2016-02-23">the 23<span class="ordinal">rd</span>of February 2016.</time>
   ```

   这样可以通过vertical-align将其设置为上标文本，即使用super关键字（字号也小一些）：

   ```css
   .ordinal{
   	vertical-align: super;
   	font-size: smaller;
   }
   ```



## 文本粗细

使用font-weight属性可设置标题文本的粗细。有些字体的变体很多，此时，可以不用给出变体的名字，而只使用关键字：normal、bold、bolder和lighter。也可以直接给出数字值，都是100的整数倍：100、200、300、400等等，最大为900。



默认值normal对应400，bold对应700，这两个粗细值是最常用的。关键字bolder和lighter的工作机制略有不同，它们的作用是在继承值的基础上把文本变粗或变细。



作为标题的默认值，这里使用了数字值500，而对h1和h2分别使用了超粗和中粗：

```css
h1,h2,h3,h4,h5,h6{
	font-weight: 500;
}

h1{
	font-weight: 800;
}

h2{
	font-weight:600;
}
```





# 行长、律动和毛边

接下来探讨一个对阅读体验有着重大影响的因素：行长。过长或过短的文本会打断人的眼球移动，导致读者无法连续阅读，最后甚至读不下去。



主体内容的文本行长通常是45-75个字符，平均值为66个字符。排版专家发现这个建议同样适用于今天的网页，特别是大屏幕中的网页。对于小屏幕而言，行长至少也应该有40个字符。



要控制行长，可以通过设定包含文本的段落、标题等元素的宽度来实现。



对于页面主体文本而言，Georgia字体的字母相对较宽，因此行长就要考虑使用前述范围的上限。为此，简单地将article元素的宽度设置为36em（平均每个字符0.5em），并令其在页面上居中。如果视口缩小到比这个值更窄，该元素会自动调整宽度。

```css
article{
	max-width: 36em;
	margin: 0 auto;
}
```



## 文本缩进与对齐

默认情况下，文本是左对齐的。文本左对齐有助于眼睛找到下一行，保持阅读节奏。对于连续的段落，或者为相邻段落设置一行的外边距，或者设置首段缩进。在此选择后一种方案，使用相邻组合符设置text-indent属性：

```css
p + p{
	text-indent: 1.25em;
}
```

**相邻兄弟选择器** (`+`) 介于两个选择器之间，当第二个元素*紧跟在*第一个元素之后，并且两个元素都是属于同一个父elment的子元素，则第二个元素将被选中。



这段代码将对相邻文本中的第二段文本设置文本缩进。



同时再居中显示h1元素，以及给它添加一个底部边框，从而让它能跟下面的文本连成一体：

```css
h1{
	text-align: center;
	border-bottom: 1px solid #c8bc9d;
}
```

text-align属性可以接受下列任意一个关键字值：left、right、center和justify。



## 多栏文本

把整篇文章宽度都限制在36em可以达到限制行长的目的，但是对于大屏幕而言，却又太浪费空间了，留着大片的空白很可惜。有时候，为了有效利用宽屏，可以把文本分成多栏，并对每栏的宽度加以限制。



如果把之前设置的max-width增加到70em，那么可以分成3栏。为此，要把columns属性设置为想要的最小宽度。栏间距通过column-gap属性控制：

```css
article{
	max-width: 70em;
	columns: 20em;
	coumn-gap:1.5em;
	margin: 0 auto;
}
```

这里的columns属性是column-count和column-width属性的简写形式。如果只设置column-count属性，浏览器会严格生成指定数量的栏，不管宽度如何。如果同时设置了column-count和column-width，则前者会作为最大栏数，后者会作为最小栏宽。



```css
columns: 20em; /* 在保证最小宽度20em的前提下，自动设置栏数 */

column-width: 20em; /* 同上 */

 columns:3 /* 3栏，自动设置宽度 */
 
 columns-count: 3; /* 同上 */
 
 columns: 3 20em; /* 至少3栏，每栏宽度至少20em */
 
 /* 以下两条声明的组合相当于以上代码的简写形式 */
 column-count: 3;
 column-width: 20em;
```





