---
title: "精通CSS读书笔记(1)——基础知识" 
date: 2023-05-22T22:32:39+08:00
draft: false
tags: ["web","css"]
categories:
  - "web"
  - "css"
toc: true
---



# 组织代码



## HTML版本



## 应该使用哪个版本

对于CSS属性，可以访问“can i use”网站（[caniuse](https://caniuse.com)）。这个网站可以搜索属性或属性组，结果配有统计信息，显示支持它们的浏览器百分比，包括桌面浏览器和移动浏览器。另一个非常有想法的项目是[webplatform](https://webplatform.github.io/)，是W3C和几家浏览器厂商及行业巨头搞出来的，目标是收集合并它们所有关于CSS、HTML、Javascript API等支持情况的文档。不过，就跟很多大型项目一样，最终要完成那么庞大的Web技术文档的聚合，需要花很长时间。此外，Mozzila的开发者文档，即MDN，也是一个非常好的参考。



## 渐进增强

平衡向后兼容性与最新的HTML和CSS特性，涉及一种叫作渐进增强的策略。所谓渐进增强，大意就是“首先为最小公分母准备可用的内容，然后再为支持新特性的浏览器添加更多交互优化”。听起来有点复杂，而实际上HTML和CSS的实现已经部分内置了这一策略。

对HTML而言，这意味着浏览器在遇到未知元素或属性时并不会报错，而且也不会对页面产生什么影响。比如，可以在页面里使用HTML5定义的新input元素。假设表单中有一个电子邮件的标记如下：

```html
<input type="text" id="field-email" name="field-email">
```

要使用新的input元素，应该把type属性改成这样：

```html
<input type="email" id="field-email" name="field-email">
```

尚未实现这个新字段类型的浏览器碰到它只会想：这是啥意思呀？不明白。然后回退为默认的text类型，结果和上面的第一行代码一样。而实现了这个类型的新浏览器则知道email想让用户在这里填写什么样的数据。假如还在这里使用了内置的表单验证，那么支持它的新浏览器也会帮忙做验证。这样，既渐进增强了页面，也不会对旧版本浏览器产生不好的影响。



CSS中的渐进增强同样也反映在浏览器如何对待新属性上。任何浏览器无法识别的属性或值都会导致浏览器丢弃相应的声明。因此，只要同时提供合理的后备声明，使用新属性就不会带来不良后果。



举个例子，很多现代浏览器支持以rgba函数方式表示的颜色值。这种方式可以分别传入红、绿、蓝通道，以及阿尔法（alpha，即透明度）通道的值。可以这样使用它：

```css
.overlay{
	background-color: #000;
	background-color: rgba(0,0,0,0.8);
}
```

这条规则定义了类名为overlay的元素背景为黑色，但随后又用rgba声明背景色应稍微透明。如果浏览器不支持rgba，那么相应元素的背景色就是不透明的黑色。如果浏览器支持rgba，那么第二条声明就会覆盖第一条。也就是说，即使并非所有浏览器都支持rgba，我们也可以使用它，只是要先为它声明合适的后备代码。



1. 厂商前缀

   浏览器厂商也基于相同的原理为自家浏览器引入实验性特性。实验性特性的标准名称前面会加上一个特殊字符串，这样它们自己的浏览器就能识别该特性，而其他浏览器则会忽略掉该特性。比如：

   ```css
   .myThing{
   	-webkit-transform: translate(0,10px);
   	-moz-transform: translate(0,10px);
   	-ms-transform: translate(0,10px);
   	transform: translate(0,10px);
   }
   ```

   这里使用了几个不同的前缀，给相应的元素应用了变换。以-webkit-开头的适用于基于Webkit的浏览器，如Safari。Chrome和Opera都基于Blick引擎，而Blink最初也是基于Webkit开发的，所以-webkit-前缀通常也适用于这3个浏览器。-moz-前缀适用于基于Mozilla的浏览器，如Firefox。-ms-前缀则适用于微软的IE。

   

   最后又加了一条不带前缀的声明，这样那些支持标准属性名称的浏览器就不会漏网了。

2. 条件规则与检测脚本

   如果希望根据浏览器是否支持某个CSS特性来提供完全不同的样式，那么可以选择@support块。这个特殊的代码块称为条件规则，它会检测括号中的声明，并且仅在浏览器支持该声明的情况下，才会应用块中的规则：

   ```CSS
   @support(display:grid){
   	/* 在支持网格布局的浏览器中要应用的规则 */
   }
   ```



# 创建结构化、语义化富HTML

语义化标记是优秀HTML文档的基础。语义就是以系统方式表示的含义。对于根据一个形式符号的集合人工创造出的语言（比如HTML语言，及其元素和属性）来说，语义指的就是通过使用某个符号想要表示的含义。简而言之，语义化标记意味着在正确的地方使用正确的元素，从而得到有意义的文档。



结构良好的标记也能让页面在搜索引擎中排名更靠前。对于CSS来说更重要的是，有意义的标记本身为添加样式提供了方便。实际上，编写CSS的最新实践都建议先给网站一组“基础”样式。



## ID和class属性

有意义的元素提供了不错的基础，却没有提供应用视觉效果所必需的全部“接入点”。除了元素本身，还需要一种方式把样式“接入到”文档上，这就是ID和class属性。



给属性起名字非常重要，起什么名字意味着它是什么，或者应该怎么使用它，例如：

```html
<ul class=product-list>
	<li><a href="/product/1">Product1</a></li>
	<li><a href="/product/2">Product2</a></li>
	<li><a href="/product/3">Product3</a></li>
</ul>
```

先利用css属性在文档中创造一个product-list模块。在css里，用类名来定义一类事物。这里的product-list就意味着它可以是任何商品列表。换句话说，为product-list写好样式后，不仅可以用在这里，还可以用在网站的任何地方。



给元素添加类名时，即使类名明确用于样式，也不要体现出其视觉效果。正确的做法是让类名表示组件的类型。比如这里的类名是product-list，而非泛泛的large-centered-list。



id和class最主要的区别是ID只能应用到页面中的一个元素。也就是说，不能像product-list那样使用ID把页面中的模块定义为可重用的“模板”。ID应该用来标识特定模块的特定实例，比如：

```css
<ul id="primary-product-list" class=product-list>
	<li><a href="/product/1">Product1</a></li>
	<li><a href="/product/2">Product2</a></li>
	<li><a href="/product/3">Product3</a></li>
</ul>
```

这是product-list的一个实例，它因为有同样的class属性而获得了相应的样式。但在这里，这个实例也被ID定义为primary-product-list。每个页面通常只能有一个主要商品的列表，因此这个ID值还是比较恰当的。利用这个ID，可以为这个模块实例添加额外的样式，可以增加一些JavaScript交互，还可以作为页内导航的目标。



实际开发中，一般不建议把ID属性作为CSS的“接入点”。利用类来添加样式往往能够让代码更简单也更容易维护。ID可以用于在文档中标识元素，但通常不用于添加样式。



## 结构化元素

HTML5新增了一批结构化元素：

- section
- header
- footer
- nav
- article
- aside
- main

增加这些新元素是为了在HTML文档中创建逻辑性区块。它们可以用于包含独立内容（article）、导航组件（nav）、特定区块的头部（header），等等。其中，main元素是最新增加的，用于高亮页面中包含主要内容的区域。



除了main之外，所有其他新元素都可以在一个文档中多次出现，以便让机器人和人更好地理解文档。在HTML5引入这些新元素之前，经常能看到带有类似类名的div元素，如下：

```html
<div class="article">
	<div class="header">
		<h1>How I became a CSS Master</h1>
	</div>
	<p>Ten-thousand hours.</p>
</div>
```

其中的div元素对文档而言并没有语义价值，只是借助类名作为添加样式的“接入点”而已。现在有了HTML5的新元素，这段标记可以改写成这样：

```css
<article>
	<header>
		<h1>How I became a CSS Master</h1>
	</header>
	<p>Ten-thousand hours.</p>
</article>
```

经过修改，这段HTML的语义得到了增强，但同时也产生了意外的副作用。此时，只能通过article和header元素来添加样式了。添加样式的CSS可能会是这样：

```css
article{
	/* 样式 */
}

article header{
	/* 其他样式 */
}
```

但article和header都可能在同一个页面中多次出现。如果确实存在这种重用的情况，那么这段CSS代码就会应用于所有其他相同的元素上，此时，更好的做法是把这两个例子结合起来：

```css
<article class="post">
	<header class="post-header">
		<h1>How I became a CSS Master</h1>
	</header>
	<p>Ten-thousand hours.</p>
</article>
```

相应的CSS规则就可以使用类名为这段标记应用样式了：

```css
.post{
	/* 样式 */
}

.post-header{
	/* 其他样式 */
}
```



## div和span

即使有了新语义元素，div元素依然不多余。在没有合适的语义元素的情况下，div仍然是给内容分组的一个不错的选择。



与div元素类似的还有span。同样，在无须表示语义、仅需添加样式的情况中，可以使用span。与div不同，span是文本级元素，可以用于在文本流中建立结构。不过在使用无语义的span之前，也一样要确保真的不需要使用任何语义元素。比如，使用time标记时间和日期，使用q标记引用，使用em标记需要强调的内容，使用strong标记需要重点强调的内容。





## 重新定义的表现性文本元素

时至今日，`<b>`和`<i>`可以算是幸存的表现型标记了，它们以前分别用于将文本标记为粗体（bold）和斜体（italic）。HTML5最终决定保留它们，但改变了它们的含义。多数情况下，应该选择使用`<em>`或`<strong>`，因为它们是用来强调及重点强调内容的语义正确的选择。





