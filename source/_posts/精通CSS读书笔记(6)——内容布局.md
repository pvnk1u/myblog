---
title: "精通CSS读书笔记(6)——内容布局" 
date: 2023-06-05T23:28:39+08:00
draft: false
tags: ["web","css"]
categories:
  - "web"
  - "css"
toc: true
---





网页是由不同内容块构成的：标题、段落、链接、列表、图片、视频等等。这些元素可以按照主题组织起来，比如一个标题，几段文本，外加一张图片就是一篇新闻报道。通过控制每个组件内部元素的位置、大小、顺序，就可以更好地传达它们的功能与含义。



所有内容块会进一步组织成整个页面的布局。下一章将会讲到如何系统化地布局页面。本章仍然聚焦于个别的内容块，从而深入理解如何对每个内容块进行布局。



之前大致讲过使用定位和浮动来实现布局，这两种手段各有千秋。此外，也可以使用表格显示模式和行内块来实现布局，当然也各有各的优缺点。CSS新增的Flexible Box Layout Module（或Flexbox）为内容块提供了一大批顺序、方向、对齐及尺寸相关的属性。Flex是非常强大的布局功能，本章会详细介绍。



# 定位

定位并不适合于总体布局，因为它会把元素拉出页面的正常流。反过来看，这也正是定位在CSS中之所以重要的原因，接下来将简单探讨一下定位在哪些情况下最合适。



以下是对定位内容的简单总结：

1. 元素的初始定位方式是静态定位（static），意思是块级元素垂直堆叠。
2. 可以把元素设置为相对定位（relative），然后可以相对于其原始位置控制该元素的偏移量，同时又不影响其周围的元素。**与此同时，这也为该元素的后代元素创造了定位上下文。**这一点也是相对定位真正的用处。以前，在一些古老的布局技巧中，经常要偏移元素，当然现在已经很少这样了。
3. 绝对定位（absolute）支持精确定位元素，相对于其最近的定位上下文：或者是其非静态定位的祖先元素，或者是HTML元素。绝对定位的元素会脱离页面流，然后再相对于其定位上下文进行定位。默认情况下，它们会被浏览器定位于之前静态定位时所处的位置但不会影响周围的元素。然后，可以相对于定位上下文来改变它们的位置。
4. 固定定位（fixed）与绝对定位基本类似，只不过**定位上下文被自动设置为浏览器视口。**



## 绝对定位的应用场景

绝对定位非常适合创建弹出层、提示和对话框这类覆盖于其他内容之上的组件。它们的位置可以通过top、right、bottom和left属性控制。关于绝对定位，了结以下知识可以写出更高效的代码。



1. 利用初始位置

   比如有一篇介绍太空飞船的文章，想在文章上添加一些行内的评注。这些评注最好以气泡图的样式显示在文章外部的空白区域，如下图所示。

   ![Spaceship](https://pvnk1u.github.io/images/Spaceship.PNG)

   每个评注组件都是一个aside组件，实际位置在它们指向的段落后面：

   ```html
   <p>This is	a fake article[...]</p>
   <aside class="comment">I`ve never done this. Is that really true?</aside>
   <p>You may think[...]</p>
   ```

   为了让评注显示在段落之后，需要使其绝对定位。而我们不必为了在垂直方向上准确的定位它们而给出确切的上偏移量。

   

   **绝对定位的元素默认会待在自己静态定位时的地方**，因此第一步是让评注显示在其初始位置。

   ```css
   .comment{
   	position: absolute;
   }
   ```

   添加这段CSS代码之后，aside元素将会变成绝对定位，即脱离页面流。这样一来，原本会跟在p元素文本段落之后看起来像是单独段落的aside内的内容将会出现在再往后的第二段p元素的上方，导致显示重叠。

   

   接下来需要把评注向左和向上偏移，把它定位到之前段落的旁边。这听起来像是要使用相对定位，但元素不能同时既是绝对定位又是相对定位。如果此时使用方向性偏移属性（top、right、left和bottom），那就既要用到定位上下文，又要设置确定的偏移量。好在不用这么麻烦，在这里完全可以通过负外边距来移动元素：

   ```css
   .comment{
   	position: absolute;
   	width: 7em;
   	margin-left: -9.5em;
   	margin-top: -2.5em;
   }
   ```

   在CSS中，负外边距是完全有效的，它们有如下有趣的行为。

   - 左边或上边的负外边距会把元素向左或向上拉，盖住其旁边的元素。
   - 右边或下边的负外边距会把相邻元素向左或向上拉，盖住设置了负外边距的元素。
   - 在浮动的元素上，与浮动方向相反的负外边距会导致浮动区域缩小，使得相邻元素盖住浮动的元素。而与浮动方向相同的负外边距会在该方向上把浮动的元素向外拉。
   - 给未声明宽度的非浮动元素应用负外边距时，左、右负外边距会向外拉伸元素，导致元素扩张，有可能盖住相邻元素。

   对评注气泡组件而言，使用左和上负外边距把元素拉到位的做法，与使用相对定位很相似。

2. 创建三角形

   在评注气泡组件中，指向前面段落的小三角形又相对于评注气泡进行了绝对定位。它是通过伪元素创建的，使用了一种很古老的基于边框的技巧。

   相关代码如下：

   ```css
   .comment:after {
     position: absolute;
     content: '';
     display: block;
     width: 0;
     height: 0;
     border: .5em solid #dcf0ff;
     border-bottom-color: transparent;
     border-right-color: transparent;
     position: absolute;
     right: -1em;
     top: .5em;
   }
   ```

   这里创建了一个0像素的块，其边框是0.5em，而且边框也只显示左、上边框。结果浏览器就只渲染出一个三角形。不用图片一样可以创造出三角形来！然后再把三角形定位到评估气泡的右上角位置。这里通过将border-bottom-color和border-right-color的值设置为transparent透明进而实现右半下角透明的效果（进而只剩下左上半角的图形）。

3. 利用偏移实现自动大小

   从另一个角度看，有必要知道：**在绝对定位的情况下，如果声明了多个或所有偏移值，那么结果会怎么样。如果没有显式声明元素大小，那么绝对定位元素的大小由自身包含内容的多少来决定。如果相对于定位上下文的各个边声明偏移值，那么元素会被拉伸以满足设定的规则。**

   

   比如，如果希望让某元素总是与其外部元素保持一定距离，但又不给任何元素设定大小。如下图所示，一张图片上有一个包含文本的元素。

   ```html
   <header class="photo-header">
   	<img src="images/big_spaceship.jpg" alt="An artist`s mockup of the "Dragon" spaceship">
   	<div class="photo-header-plate">
   		<h1>SpaceX unveil the Crew Dragon</h1>
   		<p>Photo from SpaceX on <a href="https://www.flickr.com/photos/spacexphotos/16787988882/" >
   		Flickr</a></p>
   	</div>
   </header>
   ```
   
   ![SpaceX](https://pvnk1u.github.io/images/SpaceX.PNG)



**假设不想给这个包含标题的盒子设定明确的宽度，那么可以只指定其右、下、左边的偏移，让它自己去计算上空距离**：

```css
.photo-header{
	position: relative;
}

.photo-header-plate{
	position: absolute;
	right: 4em;
	bottom: 4em;
	left: 4em;
	background-color: #fff;
	background-color: rgba(255,255,255,0.7);
	padding: 2em;
}
```

**无论图片多大，标题区始终都会位于距离底边及左、右两边4em的地方，而且会在标题折行的情况下自动调整高度，从而适应不同的屏幕大小。**



## 定位与z-index：堆叠内容的陷阱

要用好定位，还有一个重点技术必须掌握，那就是z-index，也就是堆叠元素的次序。基本原理：**静态定位（static）以外的元素会根据它们在代码树中的深度依次叠放，就像打扑克牌一样，后发的牌会压在先发的牌上面。它们的次序可以通过z-index来调整。**



设置了z-index的元素，只要值是正值，就会出现在没有设置z-index的元素上方。尚未设置z-inde的元素在z-index值为负的元素上方。



除了z-index，还有其他影响元素堆叠次序的因素。这里也有一个概念，叫堆叠上下文。就像一盒扑克牌，每张牌本身也是一个上下文（牌盒），而牌只能相对于当前的牌盒排定次序。有一个根堆叠上下文，所有z-index不是auto的定位元素都会在这个上下文中排序。随着其他上下文的建立，就会出现堆叠层级。



堆叠上下文是由特定属性和值创建的。比如，任何设定了position: absolute及值不是auto的z-index属性的元素，都会创建一个自己后代元素的堆叠上下文。



在一个堆叠上下文内部，无论z-index值多大或多小，都不会影响其他堆叠上下文，毕竟不能相对于别的堆叠上下文重新排序。



如下图所示：

![OpacityAndZIndex](https://pvnk1u.github.io/images/OpacityAndZIndex.PNG)

**容器A、B、C和D都是绝对定位的，其中C是B的子元素。容器C和D设置了z-index，但由于容器B的opacity值小于1，所以它又创建了一个新的独立的堆叠上下文。于是，就算C的z-index的值再大，它也不会跑到D的上方。**



**设置小于1的opacity值也可以创建新的堆叠上下文。opacity小于1的元素需要独立渲染（包括它的所有后代元素），然后再放到页面上。这样就可以保证在把它们放到页面上时，原有的元素不会与半透明的元素发生交错。后面会有这个例子及其他类似的例子，比如transform和filter属性，也会触发创建新的堆叠上下文。**



# 水平布局

通常，页面会随内容增加沿垂直方向扩展。后来添加的任何块容器（div、article、h1-h6等等）都会垂直堆放起来，因为**块级元素的宽度是自动计算的**。**因此，在需要给内容块设置明确宽度，并让它们水平排列时，就需要特殊处理。**



使用浮动来实现小型“媒体组件”布局是一种基础的组件模式，即组件一边是图片（或其他媒体），另一边是文本，“左边是这个，右边是那个，二者相互关联“。很多网站都在使用这种模式。如下图所示：

![MediaComponent](https://pvnk1u.github.io/images/MediaComponent.PNG)

**除此之外，还有很多通用模式的组件，可见于各类网站。其中很多都涉及水平布局。为此，CSS引入了Flexbox来专门解决水平布局问题（以及其他问题）**，但在得到浏览器完全支持之前，可能还要借助浮动、inline-block显示，或者表格显示模式，才能完美实现水平布局。



## 使用浮动

在太空飞船的例子中，有一个使用浮动的例子。figure浮动到了右侧，让行盒子对其四周环绕。同时通过margin-right的负外边距把图片向外推出去一点，以便与文本保持一些间距。

```html
<style>
    figure{
        background-color: #eee;
        margin: 0;
        padding: 1em;
        float: right;
        max-width: 17em;
        margin-right: -8em; /* 向右推出 */
        margin-left: 1em;
	}
</style>

<p>You may think[...]</p>
	<figure>
		<img src="images/spaceship.jpg" alt="The Dragon spaceship in orbit around Earth.">
		<figcaption>The "Dragon" spaceship,created by SpaceX.Image from <a href="https://www.flickr.com/photos/spacexphotos/16787988882/">Flickr.com</a></figuration>
	</figure>
<p>There`s various[...]</p>
```

![SpaceshipFloat](https://pvnk1u.github.io/images/SpaceshipFloat.PNG)



下面的代码中，删除了这个负外边距，并把插图宽度设置为文章宽度的一半，而且还增加了第二幅插图。此时，两幅插图并肩而立。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Two floated images side by side</title>
  <script src="js/html5shiv.min.js"></script>
  <style>
    body {
      font-family: Georgia, Times New Roman, serif;
      line-height: 1.5;
      padding: 2em 8em;
      max-width: 35em;
      margin: 0 auto;
    }
    h1 {
      font-family: Avenir, Franklin Gothic, sans-serif;
    }

    figure {
      background-color: #eee;
      margin: 0;
      padding: 1em;
      box-sizing: border-box;
      float: right;
      max-width: 50%;
    }
    figure img {
      width: 100%;
      display: block;
      margin-bottom: .5em;
    }
    figcaption {
      font-style: italic;
      font-size: .875em;
    }
  </style>
</head>
<body>
  <h1>A fake article about spaceships</h1>

  <p>This is a fake article about spaceships. Remember when you did presentations in middle school, and you told the class about what your assignment was about, then read aloud from the paper you had written, including the title?
    ”My assignment is about spaceships. Spaceships. Spaceships are very large, and fly in space...”.</p>
    <p>You may think that spaceships are fake, like this article, but they’re not. There are actual spaceships,
  flying in space right now, probably. For example, there’s the International Space Station, which is 
  a spaceship of sorts. Well, actually it’s a space station, which is even cooler!</p>
  <figure>
    <img src="images/spaceship.jpg" alt="The Dragon spaceship in orbit around Earth.">
    <figcaption>The ”Dragon” spaceship, created by SpaceX. Image from <a href="https://www.flickr.com/photos/spacexphotos/16787988882/">Flickr.com</a></figcaption>
  </figure>
  <figure>
    <img src="images/spaceship2.jpg" alt="The Dragon spaceship landing on Mars.">
    <figcaption>Artist mockup of Mars landing. Image from <a href="https://www.flickr.com/photos/spacexphotos/21424800115">Flickr.com</a></figcaption>
  </figure>
  <p>There’s various government organizations and companies that are building spaceships. One of them is
  SpaceX. An image of one of their spaceships appears on this very page.</p>
  <p>I can't be bothered to write any more about spaceships so the rest of the text will just be
    nonsense. This isn’t a school assignment after all.</p>
  <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Sequi saepe harum, excepturi dolorum voluptatem hic amet nemo. Incidunt tenetur dignissimos laborum molestiae reiciendis ipsum quas temporibus nisi dolor, ea libero!</p>
  <p>A assumenda, et eius odit, rerum delectus placeat dolores eveniet quis. Iure neque deserunt mollitia impedit illo corporis odio quod, velit ducimus!</p>
</body>
</html>
```

效果如下图所示：

![TwoSpaceshipImage](https://pvnk1u.github.io/images/TwoSpaceshipImage.PNG)

可以看到从第一个figure元素开始排在最右边，后面的第二个figure元素依次向左排列。



**这种布局方式是很多CSS布局会用到的一种基本技术，即让浮动的元素构成一行中的列。但正如之前讨论过的，浮动也会有一些问题。浮动的元素并不在页面流中，因此需要一个元素来包含浮动元素。为此，可以给容器内部的一个（伪）元素应用clear，也可以通过规则让容器成为一个新的块级格式化上下文（BFC，blocking formating context）。必要时，浮动也可以包含多行，但如果上面的行有浮动元素，也可能会被卡住。**



浮动也可以对有限的水平内容进行独立于源码次序的排序。比如，通过分别向左或向右浮动两个元素，可以调整两幅插图的次序。将上面代码示例中的float的值由right改为left即可实现两幅图片位置调换的效果。



因为浏览器对浮动的支持极为普遍，所以浮动也成为了各种水平布局中的常用技术。除了浮动，实际上还有其他技术可以用来创建水平布局，虽然这些技术各有利弊。



## 行内块布局

文本行自身就是水平布局时，至少在从左往右和从右往左书写的语言中是如此。使用行内元素（如span、time或a）时，它们会与文本沿相同方向水平对齐。也可以把行内块加入到文本流中，创造出水平对齐的元素，从视觉上看又是一个块。



比如，为太空飞船的文章末尾添加一些元数据，包括作者姓名、照片和电子邮件地址。为了添加样式，还使用了两个额外的span元素：

```html
<p class="author-meta">
	<img class="author-image" src="images/author.jpg" alt="Arthur C. Lark">
	<span class="author-info">
		<span class="author-name">Written by Arthur C. Lark</span>
		<a class="author-email" href="mailto:arthur.c.lark@example.com</a>">arthur.c.lark@example.com</a>
	</span>
</p>
```

现在，.author-meta段落的底部会与图片底部及文本基线对齐。段落中的所有空白字符，包括图片和作者信息之间的换行符都被渲染为空格。这些空格的宽度取决于字体及其大小。

![author-case-1](https://pvnk1u.github.io/images/author-case-1.PNG)



接下来，把图片和作者信息转换为行内块：

```css
.author-image,
.author-info{
	display: inline-block;
}
```

渲染之后，其实并没有什么可见的差别。差别只在于现在图片和作者信息都是块了。比如，可以把作者信息中包含的姓名和电子邮件地址分别列为两行，只要把它们修改为块级元素即可：

```css
.author-name,
.author-email{
	display: block;
}
```

 现在已经比较接近想要的水平布局了：左边是浮动的图片，右边是一个文本块。但还有一点，此时作者信息块最后一行的基线与图片底部是对齐的。下图展示了此时的状况：

![author-case-2](https://pvnk1u.github.io/images/author-case-2.PNG)



现在可以通过vertical-align属性相对于图片来对齐作者信息。下图展示了在对齐关键字设置为top时，作者信息块的顶部与图片顶部对齐的效果：

![author-case-3](https://pvnk1u.github.io/images/author-case-3.PNG)



1. 行内块的垂直居中

   假设想要的设计是让作者信息块相对于图片垂直居中。可能会这么写：

   ```css
   .author-info{
   	vertical-align: middle;
   }
   ```

   然而，结果可能并非是垂直居中，如下图所示：

   ![author-case-4](https://pvnk1u.github.io/images/author-case-4.PNG)

   这是因为，**关键字middle在应用给行内块时，其含义是“将这个行内块的垂直中心点与这行文本x高度的中心点对齐”。这个例子中没有行内文本，（行内最高的）图片就成为行盒子高度以及基线位置的元素。而此时x高度的中心点就在图片底部（基线）靠上一点。要想将作者信息与图片一起垂直居中，需要让这两个元素都参照同一个“中心点”：**

   ```css
   .author-image,
   .author-info{
   	vertical-align: middle;
   }
   ```

   **因为图片此时也是行内块，所以它就与作者信息在同一个垂直点上居中对齐了**，从而得到了想要的布局，如下图所示：

   ![author-case-5](https://pvnk1u.github.io/images/author-case-5.PNG)

   

   如何确定行盒子的基线，以及这些规则如何影响行内及行内块元素是比较复杂的。**对于利用行内块创建水平布局而言，如果需要垂直对齐，有以下两个要点：**

   - **要让行内块沿上方对齐（很像浮动），设置：vertical-align: top;**
   - **要让两个元素的内容垂直对齐，先把它们都转化成行内块，再对它们应用vertical-align: middle。**

2. 在容器元素中垂直居中

   前面的两个要点的第二个告诉我们：可以在任意高度的容器内垂直居中内容。其实也不完全对。唯一的前提是把容器的高度设置为确切的高度。

   

   比如，假设想把作者元数据块设置为10em高，然后在其中居中放置作者图片和信息。首先，给.author-meta块应用这个高度。为清晰起见，也添加了一个边框。如下图：

   ```css
   .author-meta{
   	height: 10em;
   	border: 1px solid #ccc;
   }
   ```

   ![author-case-6](https://pvnk1u.github.io/images/author-case-6.PNG)

   但作者信息和图片并没有相对于容器块垂直居中对齐，而是仍然沿原来那条假想的文本行对齐。**为了实现与容器垂直对齐，还需要增加一个行内块元素，让它占据100%的容器高度。这个元素会让middle关键字认为容器的垂直中点是对齐点。为此，可以借助伪元素。如下图所示，在引入了这个“幽灵元素”后，假想的基线就以它为准了。**

   ```css
   .author-meta:before{
   	content: '',
   	display: inline-block;
   	vertical-align: middle;
   	height: 100%;
   }
   ```

   ![middle-of-box](https://pvnk1u.github.io/images/middle-of-box.PNG)

   如图所示，利用高度为100%的伪元素，让middle关键字代表容器的垂直中心点。此时，就好像整个.author-meta容器中只有一行文本，且高度与容器高度相同。因为这个伪元素是一个行内块，且其垂直对齐方式设置为middle，所以其他行内块也就与容器的中心垂直对齐了。接下来要做的就是水平居中内容。因为行内块像文本一样对齐，所以这里使用text-align。

   ```css
   .author-meta{
   	height: 10em;
   	text-align: center;
   	border: 1px solid #ccc;
   }
   
   .author-info{
   	text-align: left;
   }
   ```

   结果就是.author-meta既水平居中又垂直居中，如下图所示：

   ![author-case-7](https://pvnk1u.github.io/images/author-case-7.PNG)

   确切地讲，这里的水平居中并不准确。这是因为行盒子内的任何空白符都会被渲染为一个空格。伪元素就会创建这么一个空格，导致内容向右偏移几个像素。通过给伪元素应用负外边距，可以抵消空格的宽度。

   ```css
   .author-info:before{
   	margin-right: -.25em;
   }
   ```

3. 追究细节：与空白战斗到底

   对于每个块都占据确切宽度的水平布局而言，空白是个突出的问题。下面以另一个常见的组件为例，介绍在使用行内块的情况下如何解决这个问题，尽量不使用具体数值。

   

   创建一个导航条，包含4个链接项，每一项都占据宽度的四分之一。标记如下：

   ```html
   <nav class="navbar">
   	<ul>
   		<li><a href="/home">Home</a></li>
   		<li><a href="/spaceships">Spaceships</a></li>
   		<li><a href="/planets">Planets</a></li>
   		<li><a href="/stars">Stars</a></li>
   	</ul>
   </nav>
   ```

   以下CSS标记为导航条添加了基本的颜色及字体样式，并通过轮廓线突出了链接项的边界。这里将每一项设置为占据25%的宽度，4项正好占据全部宽度。

   ```css
   nav {
     display: block;
   }
   
   .navbar ul {
     font-family: 'Avenir Next', Avenir, Corbel, 'Franklin Gothic', 'Century Gothic', CenturyGothic, AppleGothic, sans-serif;
     list-style: none;
     padding: 0;
     background-color: #486a8e;
   }
   
   .navbar li {
     text-transform: uppercase;
     display: inline-block;
     text-align: center;
     width: 25%;
     -moz-box-sizing: border-box;
          box-sizing: border-box;
     background-color: #12459e;
     outline: 1px solid #fff;
   }
   
   .navbar li a {
     display: block;
     text-decoration: none;
     line-height: 1.75em;
     padding: 1em;
     color: #fff;
   }
   ```
   
   通过使用box-sizing: border-box确保每一项的边框及内边距都包含在各自25%的宽度以内。导航条本身的背景颜色是蓝灰色，链接项的颜色是深蓝色，链接文本颜色是白色。
   
   
   
   ![multilinenav](https://pvnk1u.github.io/images/multilinenav.PNG)
   
   **HTML源代码中的换行符被渲染成了空白符，再加上每一项25%的宽度，就导致了折行。**要消灭这些空白符，可以尝试把所有li标签都排到一行，但这种要求显示不友好。
   
   
   
   解决问题的方法也很简单粗暴，就是把包含元素ul的font-size设置为0（从而让每个空格的宽度为0），然后在每一项上重新设置大小：
   
   ```css
   .navbar ul{
   	font-size: 0;
   }
   
   .navbar li{
   	font-size: 16px;
   	font-size: 1rem;
   }
   ```
   
   这样就如期解决了空白问题，每一项都相互靠拢，只占据了一行。但这个技术也有缺点。首先与可以继承的font-size有关。假设在导航条上设置的是16像素的font-size，那么就不能再使用em单位或比例，让每一项继承一个可伸缩的大小了。它会变成与0相乘。不过，可以使用rem单位，相对于根字体大小来保持可伸缩性。



## 使用表格显示属性实现布局

表格中的行恰好具有导航条例子中我们想要的特质：一组单元格恰好占满一行，而且永远不会折行。这也正是HTML表格在WEB发展早期成为页面布局垄断技术的缘由。可以通过CSS来借用表格的显示模式，不必求诸HTML表格标记。



如果将前面导航条的例子改为对ul元素使用的一种表格显示模式，并将其中的每一项设置为表格单元，那么也会得到与使用行内块一样的效果：

```css
.navbar ul{
	width: 100%;
	display: table;
	table-layout: fixed;
}

.navbar li{
	width: 25%;
	display: table-cell;
}
```

这样可以得到相同的布局效果。



**注意这里将ul元素的宽度设置为100，是为了保证导航条能扩展到与父元素同宽。与常规块不同，不设置宽度的表格会“收缩适应”内容宽度，除非包含内容的单元把它撑开，让它的宽度足以填充父容器。**



表格行中每一列的宽度有两种算法。默认情况下，浏览器会使用“自动”算法。这是一种没有明确规定，但某种程度上又是事实标准的算法，基本上就是根据自身单元格内容所需的宽度来决定整个表格的宽度。



另一种算法是“固定”表格布局，即使用table-layout: fixed。这种算法下的列宽由表格第一行的列决定。第一行中声明的列宽具有决定性，后续行如果遇到内容较多的情况，只能折行或者溢出。



在利用表格显示模式来创建布局时，必须清楚这样也会引入表格的问题。比如，渲染为表格单元的元素无法应用外边距，给表格单元应用定位时的行为也无法预料。



**表格单元格中的垂直对齐**

使用表格显示模式时，表格单元格中的垂直对齐效果无须借助任何额外的技术。只要给显示为table-cell的元素应用vertical-align: middle，就可以令其中的内容在单元格中垂直居中。







# Flexbox

Flexbox，也就是Flexible Box Layout模块，是CSS提供的用于布局的一套新属性。这套属性包含针对容器（弹性容器，flex container）和针对其直接子元素（弹性项，flex item）的两类属性。Flexbox可以控制弹性项的如下方面：

1. 大小
2. 流动方向，水平还是垂直，正向还是反向
3. 两个轴向上的对齐与分布
4. 顺序，与源代码中的顺序无关



Flexbox就是为了解决行内块、浮动和表格格式对应的棘手问题而生的。



## 浏览器支持与语法

