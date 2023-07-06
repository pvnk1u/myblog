---
title: "精通CSS读书笔记(6)——内容布局" 
date: 2023-07-01T23:28:39+08:00
draft: false
tags: ["web","css"]
categories:
  - "web"
  - "css"
toc: true
---







本章介绍布局的系统性方法。上一章探讨的是个别页面组件的布局方式。了解页面布局最好先从个别组件开始。但与此同时，会发现总体结构中有重复出现的模式。本章主要讨论如何以可重用的方式实现这些结构，也就是能够盛放内容的容器。



创建容器的时候，一般都需要一个包含预置尺寸和比例的网格系统。首先会介绍相对传统的创建网格系统的方法，然后演进到使用Flexbox。后半章还会涉及到面向未来的CSS Grid Layout规范。





在把设计方案转换成模板时，很多时候想都不想，就开始写HTML和写CSS。这样写着写着，很快就会发现没有了任何回旋余地。如果能在事前稍微作一番规划，将来很可能就避免了大量的无用功。规划阶段的关键就在于从设计方案中找出重复的模式，并识别出一些本质的东西。



# 网格

说到一个网站的整体布局，经常会想到网格系统。网格系统是设计师在切分布局时作为参照的一组行和列。行和列之间的空白叫作空距。网格系统是页面布局常用的参考系。



## 布局辅助类

类名用于为布局添加样式。对于简单的网站，几个类名就够用了。比如，用于控制两栏博客布局的类名大概这样就可以了：

```css
.main-content{}
.secondary-content{}
```

随着网站的复杂度提高，会发现一些规律：由于某些部分从属于特定的内容层级，类名无法清晰地传达其意图。这时候类名重用就成了一个问题。为了让样式可以重用，很多人尝试“可视化”的命名方式，比如：

```css
.column{ /* 一般列的样式 */}
.column-half{/* 占行宽的一半 */}
.row{/*一般行的样式 */}
```

这几个类名严格来说是表现性的，也就是在HTML中要加入表现性信息。另外，这样命名一目了然，方便重用，可以一次到位地解决布局问题。



另一种做法是把具有共用样式的选择符集中到一起：

```css
.thing,
.other-thing,
.third-thing,
.fourth-thing{
	/* 共用的样式 */
}
```

这样做的好处是不必为了应用这些样式而专门搞一个类名，只需要在这一个地方添加或删除即可。但如此一来，选择符可能会越来越多，变得难以维护。这也会给代码组织带来问题。而且像这样根据共用样式而非可重用组件来分割样式，还会让修改网站中特定部分的样式变得非常麻烦，因为不得不在很多样式块之间跳来跳去。



命名规范是高质量代码的重要组成部分，把表现与标记混在一起也是权衡的结果。本章将走一条中间路线，一方面会使用一些辅助类，另一方面会尽可能少地与表现绑定起来。这是创建布局系统的简便方式，可以快速实现原型，同时还能保持样式的一致性。



## 固定、流动还是弹性

“固定布局”、“流动布局”、“弹性布局”的说法指的是在某种布局下如何约束元素的尺寸。

1. 固定布局

   指页面具有特定的宽度，比如960像素。固定布局已经流行很长时间了，因为这样设计师和开发者会轻松很多。但是，有人会质疑到底什么尺寸是最好的：现在用户屏幕的主流宽度是1024像素，还是1280像素呢？

2. 弹性布局

   指布局元素的尺寸使用em单位。这样，即使用户缩放文本大小，布局的比例也不会变。再与最小和最大宽度结合使用，还能使页面更好地适应屏幕大小。虽然弹性布局有点过时了，但其利用最大宽度限制em单位的思想是创建流动布局的关键。

3. 流动布局

   也称为“流式布局”，指页面元素会按比例缩放，但元素与元素之间的比率（有时候连元素之间的距离也）保持不变。这其实是Web的默认模式，即块级元素没有预置的宽度，其尺寸会随可用空间大小而变化。

建议尽量不要使用固定布局，最好使用流式布局，使其适应不同的设备。这种让设计能响应环境的设计方法叫作响应表WEB设计。



# 创建灵活的页面布局

本节会介绍几种实用的布局方法，可以用来创建可靠、灵活、可重用的页面布局。



接下来会创建一个如下图所示的页面布局，这个布局会随屏幕大小而展示不同的列数，同时水平的分节也会变化。

![page-layout](https://pvnk1u.github.io/images/page-layout.PNG)



把这个页面的布局简化成线框图，可以得到如下图所示的样子：

![page-layout-simple](https://pvnk1u.github.io/images/page-layout-simple.PNG)

这个线框图没有表现出布局整体是居中的，有一个最大宽度限制。接下来就从这个限制宽度包装元素开始。



## 包装元素

包装元素是页面布局中常用的一个盛放内容的元素，比如：

```html
<body>
  <!-- Some temporary style attributes added, just to see the effects of the wrapper. -->
  <div class="wrapper" style="background-color: #ccc; min-height: 300px;">
    <h1 style="margin: 0">My page</h1>
  </div>
</body>
```

下面为这个包装元素添加一些样式。这些样式通过自动外边距，将包装元素设置为在页面上居中，同时使用了最大宽度。对于流动布局而言，适应百分比来设置一个稍微小于100%的宽度是很常见的。最大宽度则相对于文本大小来设置，单位是em。

```css
.wrapper{
	width: 95%;
	max-width: 76em;
	margin: 0 auto;
}
```

body元素默认是有外边距的，为避免不必要的困扰，这里需要去掉它。简单地去掉body的外边距：

```css
body{
	margin: 0;
}
```

下图所示的结果就是布局的起点了。以上寥寥几行CSS样式反映了如下几个布局选择。

1. 主包装元素正常情况下应该是视口宽度的95%。
2. 通过简写的margin: 0 auto，我们去掉了它的上下外边距，然后将水平剩余空间平均分配给左、右外边距（每一侧是2.5%），这样就让它在页面上居中了。
3. 但是，这个包装元素最宽不会超过76em。按照每em默认为16像素计算，相当于1216像素。不过，如果用户通过浏览器缩放文本大小，包装元素的宽度也会同比例缩放。76em不是谁规定的，只是觉得它比较适合当前的布局。



![basic-layout](https://pvnk1u.github.io/images/basic-layout.PNG)



这里屏蔽了一些不可控因素，比如屏幕大小及用户的字体设置，因为不希望布局的宽度值固定。



但一个居中的布局两侧应该留出一些空白区域，这跟屏幕大小无关。我们希望布局的宽度有一个上限，以防止文本行的长度超出可读范围。假如用户的浏览器有不同的默认文本大小，那么布局的最大宽度应该也能自动缩放。



由于设计不同，选择的度量方式也可能不同，但原理都是一样的：首先大体上确定内容包装元素的限制条件，但又不能把它们的值写死。要让布局能随机应变。



“随机应变”是软件设计的各种领域中经常能听到的一句话。在整体布局时不使用特定的像素值，就可以实现这一点。而这些样式一旦写完，就可以应用给任何包装元素，只要给它们添加相应的类即可。



换句话说，现在只要应用wrapper类就行了。以下代码中有3个地方应用这个类。首先是页头部分，然后是导航条。这两个元素本身是与浏览器视口一样宽的，但通过在它们内部包含一个包装元素，就可以实现内容在布局层面上居中。这两个块之后的main元素也是包装元素，用于盛放特定于页面的内容。

```html
<header class="masthead">
	<div class="wrapper">
		<h1>Important News</h1>
	</div>
</header>
<nav role="navigation" class="navbar">
    <div class="wrapper">
      <ul class="navlist">
        <li><a href="#">Home</a></li>
        <li><a href="#">World</a></li>
        <li><a href="#">Local</a></li>
        <li><a href="#">Sports</a></li>
      </ul>
    </div>
  </nav>
<main class="wrapper">
	<!-- 这里是主体内容 -->
</main>
```

相关CSS代码如下：

```css
/* here's our wrapper */
.wrapper {
  width: 95%;
  max-width: 76em;
  margin: 0 auto;
}

/* masthead styling */
.masthead {
  background-color: #8E3339;
}
.masthead h1 {
  margin: 0;
  padding: 0.5em 0;
  color: #fff;
  text-shadow: -.1em .1em 0 rgba(0,0,0,0.3);
}

/* navbar styling */
.navbar {
  background-color: #5E2126;
  margin-bottom: 1.375em;
}

nav {
  display: block;
}
.navbar ul {
  font-family: 'Avenir Next', Avenir, Corbel, 'Franklin Gothic', 'Century Gothic', CenturyGothic, AppleGothic, sans-serif;
  list-style: none;
  padding: 0;
  margin: 0;
  background-color: #752A2F;
  display: flex;
  overflow: hidden;
}
.navbar li {
  float: left;
  text-transform: uppercase;
  text-align: center;
  box-sizing: border-box;
  flex: 1 1 auto;
  border-left: 1px solid #8E3339;
}
.navbar li:first-child {
  border-left: 0;
}
.navbar li a {
  display: block;
  text-decoration: none;
  line-height: 1.75em;
  padding: 1em 2em;
  color: #fff;
}
```



效果如下图所示：

![grid-demo](https://pvnk1u.github.io/images/grid-demo.PNG)



## 创建行容器和列

创建好了头部菜单之后，再来实现“内容“部分在水平方向上的分组。效果如下图所示：

![layout-demo-section](https://pvnk1u.github.io/images/layout-demo-section.PNG)



完整代码如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Finished column layout sketch</title>

  <!-- the base styles and "housekeeping" styles are in here: -->
  <link rel="stylesheet" href="css/grid-base.css">
<!-- the HTML5 shiv, to help older browsers understand styling
 on newer HTML5 elements: -->
 <script src="js/html5shiv.min.js"></script>
 <style>
  /* our grid styles: */
  .row:after {
    content: '';
    display: block;
    clear: both;
  }
  .row-quartet > * {
    width: 25%;
  }
  .row-trio > * {
    width: 33.3333%;
  }
  .col {
    float: left;
    -moz-box-sizing: border-box;
         box-sizing: border-box;
    min-height: 100px;
    outline: 1px solid #666;
  }
  .subcategory-featured {
    width: 50%;
  }
  .subcategory-content {
    width: 80%;
  }
  .subcategory-header {
    width: 20%;
  }
</style>
</head>
<body>
  <header class="masthead">
    <div class="wrapper">
      <h1>Important News</h1>
      
    </div>
  </header>

  <nav role="navigation" class="navbar">
    <div class="wrapper">
      <ul class="navlist">
        <li><a href="#">Home</a></li>
        <li><a href="#">World</a></li>
        <li><a href="#">Local</a></li>
        <li><a href="#">Sports</a></li>
      </ul>
    </div>
  </nav>

  <main class="wrapper">
    <section class="subcategory">
      <div class="row">
        <header class="col subcategory-header">
          <h2>Sub-section 1</h2>
        </header>
        <div class="col subcategory-content">
          <div class="row row-quartet">
            <div class="col subcategory-featured">
              <h3>Story</h3>
            </div>
            <div class="col">
              <h3>Story</h3>
            </div>
            <div class="col">
              <h3>Story</h3>
            </div>
          </div>
          <div class="row row-quartet">
            <div class="col">
              <h3>Story</h3>
            </div>
            <div class="col">
              <h3>Story</h3>
            </div>
            <div class="col">
              <h3>Story</h3>
            </div>
            <div class="col">
              <h3>Story</h3>
            </div>
          </div>
        </div>
      </div>
    </section>

    <section class="subcategory">
      <div class="row">
        <header class="col subcategory-header">
          <h2>Sub-section 1</h2>
        </header>

        <div class="col subcategory-content">
          <div class="row row-trio">
            <div class="col">
              <h3>Story</h3>
            </div>
            <div class="col">
              <h3>Story</h3>
            </div>
            <div class="col">
              <h3>Story</h3>
            </div>
          </div>
        </div>
      </div>
    </section>

  </main>    
</body>
</html>
```

引用的grid-base.css中的样式表代码：

```css
body {
  margin: 0;
  line-height: 1.375;
  font-family: Georgia, Times New Roman, Times, serif;
}
h1,h2,h3,h4,h5,h6 {
  font-family: Avenir Next, Avenir, Franklin Gothic, Trebuchet MS, Arial, sans-serif;
  margin-top: 0;
}
a {
  color: #8E3339;
  text-decoration: none;
}
a:hover,
a:focus {
  text-decoration: underline;
}
/* here's our wrapper */
.wrapper {
  width: 95%;
  max-width: 76em;
  margin: 0 auto;
}
/* masthead styling */
.masthead {
  background-color: #8E3339;
}
.masthead h1 {
  margin: 0;
  padding: 0.5em 0;
  color: #fff;
  text-shadow: -.1em .1em 0 rgba(0,0,0,0.3);
}

/* navbar styling */
.navbar {
  background-color: #5E2126;
  margin-bottom: 1.375em;
}

nav {
  display: block;
}
.navbar ul {
  font-family: 'Avenir Next', Avenir, Corbel, 'Franklin Gothic', 'Century Gothic', CenturyGothic, AppleGothic, sans-serif;
  list-style: none;
  padding: 0;
  margin: 0;
  background-color: #752A2F;
  display: flex;
  overflow: hidden;
}
.navbar li {
  float: left;
  text-transform: uppercase;
  text-align: center;
  box-sizing: border-box;
  flex: 1 1 auto;
  border-left: 1px solid #8E3339;
}
.navbar li:first-child {
  border-left: 0;
}
.navbar li a {
  display: block;
  text-decoration: none;
  line-height: 1.75em;
  padding: 1em 2em;
  color: #fff;
}
```



接下来分析这份代码，在这段代码中，首先创建了行容器（class名为row）及行容器中的多列内容。首先定义HTML元素及类名（行为row，列为col），然后实现了内容在水平方向上的分组。

```css
.row:after{
	content: '',
	display: block;
	clear: both;
	height: 0;
}
```

这里使用伪类和clear:both实现.row行元素分别各占一行的效果。



实现了行元素之后，再来实现创建列的效果。上一章的几种水平布局方法中，浮动是最常用的，也是浏览器支持最好的技术。因此，这里用浮动创建列。对于从左向右书写的语言，默认的向左浮动应该是最佳选择。



考虑到将来可能会在不影响列宽度的前提下，直接给列容器添加边框和内边距，还应该把box-sizing属性设置为border-box。

```css
.col{
	float: left;
	box-sizing: border-box;
    min-height: 100px;
    outline: 1px solid #666;
}
```

其中的min-height是为列指定了最低高度。outline则是指定了列的轮廓线（因为不影响元素尺寸，所以轮廓线适用于元素可视化布局和调试布局）。



接下来需要给列设置列宽。很多CSS库都使用直接表示宽度的类来指定列宽，比如：

```css
.col-1of4{
	width: 25%;
}
.col-1of2{
	width: 50%;
}
```

这种方式非常适合面向台式电脑或笔记本电脑的快速原型。根据前面定义的规则，很容易在HTML中定义一个3列的、最左列占一半宽度的布局。

```html
<div class="row">
	<div class="col col-1of2"></div>
	<div class="col col-1of4"></div>
	<div class="col col-1of4"></div>
</div>
```

这种方式的缺点是过分强调某种布局。如果将来需要根据屏幕大小动态调整布局，这种命名方式就不太合适了。



如果想通过可重用的类名来控制尺寸，就必须让HTML标记与CSS表现有一个结合点。可以给这个结合点换个名字，不使用特定的宽度或者比率，让它更为普适。用音乐来比喻的话，可以创建一条规则，让行容器在正常情况下包含4个宽度相等的部分（quartet，四重奏）。

```css
.row-quartet > * {
	width: 25%;
}
```

然后使用通用选择符，直接针对行容器的子元素，同时可以降低这条通用规则的特殊性。因为通用选择符的特殊性为0，所以后面可以用一个特殊的类名来覆盖这个宽度。此时通过以下标记就可以创建一个包含4个等宽列的行：

```html
<div class="row row-quartet">
	<div class="col"></div>
	<div class="col"></div>
	<div class="col"></div>
    <div class="col"></div>
</div>
```

这样，.row-quartet中的列如果想改变宽度，就可以应用覆盖宽度的一个类名，但这个类名并不与布局相关。于是前面的3列布局就可以这样写：

```html
<div class="row row-quartet">
	<div class="col my-special-column"></div>
	<div class="col"></div>
	<div class="col"></div>
</div>
```

```css
.my-special-column{
	width: 50%;
}
```



除了四重奏，当然还应该有三重奏：

```css
.row-quartet > *{
	width: 25%;
}
.row-trio > *{
	width: 33.3333%;
}
```

在前面的效果图中，两个子分类都有一个标题区，占整个布局区（包含标题区和内容区两块）的五分之一，右侧的内容区占剩下的五分之四。而在第一个子分类中，还有一个更大的文章列，占内容区的50%。相关代码如下：

```css
.subcategory-featured {
	width: 50%;
}

.subcategory-content {
	width: 80%;
}

.subcategory-header {
	width: 20%;
}
```

HTML代码如下：

```html
<section class="subcategory">
  <div class="row">
    <header class="col subcategory-header">
      <h2>Sub-section 1</h2>
    </header>
    <div class="col subcategory-content">
      <div class="row row-quartet">
        <div class="col subcategory-featured">
          <h3>Story</h3>
        </div>
        <div class="col">
          <h3>Story</h3>
        </div>
        <div class="col">
          <h3>Story</h3>
        </div>
      </div>
      <div class="row row-quartet">
        <div class="col">
          <h3>Story</h3>
        </div>
        <div class="col">
          <h3>Story</h3>
        </div>
        <div class="col">
          <h3>Story</h3>
        </div>
        <div class="col">
          <h3>Story</h3>
        </div>
      </div>
    </div>
  </div>
</section>

<section class="subcategory">
  <div class="row">
    <header class="col subcategory-header">
      <h2>Sub-section 1</h2>
    </header>

    <div class="col subcategory-content">
      <div class="row row-trio">
        <div class="col">
          <h3>Story</h3>
        </div>
        <div class="col">
          <h3>Story</h3>
        </div>
        <div class="col">
          <h3>Story</h3>
        </div>
      </div>
    </div>
  </div>
</section>
```



至此便实现了图中的效果。在这些定义好的网格类基础上，很容易组合和扩展出更复杂的布局模式。下面在每个容器中添加一些示例内容，以充实细节。



以下是带图片文章的代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Float grid with dummy content</title>

  <!-- the base styles and "housekeeping" styles are in here: -->
  <link rel="stylesheet" href="css/grid-base.css">
<!-- the HTML5 shiv, to help older browsers understand styling
 on newer HTML5 elements: -->
 <script src="js/html5shiv.min.js"></script>
 <style>
  /* grid styling */
  .row {
    margin: 0 -.9%;
    padding: 0;
    list-style: none;
  }
  .row:after {
    content: '';
    display: block;
    clear: both;
  }
  .col {
    float: left;
    -moz-box-sizing: border-box;
         box-sizing: border-box;
  }
  .col:last-child {
    float: right;
  }
  .row-quartet > * {
    width: 25%;
  }
  .row-trio > * {
    width: 33.33333%;
  }

  /* content styling */

  .subcategory {
    margin-top: 1.5em;
    border-bottom: 1px solid #8e3339;
  }
  .subcategory-featured {
    width: 50%;
  }
  .subcategory-content {
    width: 80%;
  }
  .subcategory-header {
    width: 20%;
  }


  .story {
    padding: .6875em;
    background-color: #eee;
  }
  .story img {
    width: 100%;
  }

</style>
</head>
<body>
  <header class="masthead">
    <div class="wrapper">
      <h1>Important News</h1>
    </div>
  </header>

  <nav role="navigation" class="navbar">
    <div class="wrapper">
      <ul class="navlist">
        <li><a href="#">Home</a></li>
        <li><a href="#">World</a></li>
        <li><a href="#">Local</a></li>
        <li><a href="#">Sports</a></li>
      </ul>
    </div>
  </nav>
  <main class="wrapper">
    
    <section class="subcategory">
      <div class="row">
        <header class="col subcategory-header">
          <h2>Lorem ipsum</h2>
        </header>
        <div class="col subcategory-content">
          <div class="row row-quartet">
            <div class="col subcategory-featured">
              <article class="story">
                <img src="http://placehold.it/600x300" alt="Dummy image">
                <h3><a href="#">Cras suscipit nec leo id.</a></h3>
                <p>Autem repudiandae aliquid tempora quos reprehenderit architecto, sequi repellat.</p>
              </article>
            </div>
            <div class="col">
              <article class="story">
                <img src="http://placehold.it/600x300" alt="Dummy image">
                <h3><a href="#">Perferendis, ipsam!</a></h3>
                <p>Neque magnam vero obcaecati facere nobis sint dolore accusamus vitae consequuntur ad necessitatibus, laborum molestiae.</p>
              </article>
            </div>
            <div class="col">
              <article class="story">
                <img src="http://placehold.it/600x300" alt="Dummy image">
                <h3><a href="#">Curabitur mattis purus nec velit.</a></h3>
                <p>Neque magnam vero obcaecati facere nobis sint dolore accusamus vitae consequuntur ad necessitatibus, laborum molestiae.</p>
              </article>
            </div>
          </div>
          <div class="row row-quartet">
            <div class="col">
              <article class="story">
                <h3><a href="#">Perferendis, ipsam!</a></h3>
                <p>Neque magnam vero obcaecati facere nobis sint dolore accusamus vitae consequuntur ad necessitatibus, laborum molestiae.</p>
              </article>
            </div>
            <div class="col">
              <article class="story">
                <h3><a href="#">Aliquam mattis eros id posuere.</a></h3>
                <p>Neque magnam vero obcaecati facere nobis sint dolore accusamus vitae consequuntur ad necessitatibus, laborum molestiae.</p>
              </article>
            </div>
            <div class="col">
              <article class="story">
                <h3><a href="#">Proin leo felis, semper nec</a></h3>
                <p>Neque magnam vero obcaecati facere nobis sint dolore accusamus vitae consequuntur ad necessitatibus, laborum molestiae.</p>
              </article>
            </div>
            <div class="col">
              <article class="story">
                <h3><a href="#">Aliquam vitae risus tortor. Sed!</a></h3>
                <p>Neque magnam vero obcaecati facere nobis sint dolore accusamus vitae consequuntur ad necessitatibus, laborum molestiae.</p>
              </article>
            </div>
          </div>
        </div>
      </div>
    </section>

    <section class="subcategory">
      <div class="row">
        <header class="col subcategory-header">
          <h2>Dolor sit amet</h2>
        </header>
        <div class="col subcategory-content">
          <div class="row row-trio">
            <div class="col">
              <article class="story">
                <img src="http://placehold.it/600x300" alt="Dummy image">
                <h3><a href="#">Ut sit amet mi massa</a></h3>
                <p>Neque magnam vero obcaecati facere nobis sint dolore accusamus vitae consequuntur ad necessitatibus, laborum molestiae.</p>
              </article>
            </div>
            <div class="col">
              <article class="story">
                <h3><a href="#">Nunc mollis sit amet nunc</a></h3>
                <p>Neque magnam vero obcaecati facere nobis sint dolore accusamus vitae consequuntur ad necessitatibus, laborum molestiae.</p>
              </article>
              <article class="story">
                <h3><a href="#">Duis sed ante enim. Cras</a></h3>
                <p>Neque magnam vero obcaecati facere nobis sint dolore accusamus vitae consequuntur ad necessitatibus, laborum molestiae.</p>
              </article>
            </div>
            <div class="col">
              <article class="story">
                <img src="http://placehold.it/600x300" alt="Dummy image">
                <h3><a href="#">Animi, explicabo, ipsum</a></h3>
                <p>Neque magnam vero obcaecati facere nobis sint dolore accusamus vitae consequuntur ad necessitatibus, laborum molestiae.</p>
              </article>
            </div>
          </div>
        </div>
      </div>
    </section>
  </main>
</body>
</html>
```

效果图如下所示：

![float-grid-pic-content-demo](https://pvnk1u.github.io/images/float-grid-pic-content-demo.PNG)



在列容器（类名为col的元素）中，使用了带story类名的article元素。这个额外的元素将布局与内容隔离开来，避免因加重包装元素的负担而导致其过载。



这里需要注意的是以下有关.story的CSS代码：

```css
.story {
    padding: .6875em;
    background-color: #eee;
}

.story img {
	width: 100%;
}
```

这里指定了.story元素的padding内边距的值及背景颜色，使内容区更加美观，随后设置.story的子img图片元素的宽度为100%。这里需要注意的是，例子中用的是同样一张图片，但是因为img的父元素.story的宽度不同，所以展示出的图片大小也不同，第一张图片的story的宽度是其他story元素的2倍，所以图片大小也是其他图片的两倍。



## 流式空距



