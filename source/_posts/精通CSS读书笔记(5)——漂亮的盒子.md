---
title: "精通CSS读书笔记(5)——漂亮的盒子" 
date: 2023-06-04T17:28:39+08:00
draft: false
tags: ["web","css"]
categories:
  - "web"
  - "css"
toc: true
---



# 背景颜色

先从一个非常简单的例子开始：为整个页面添加背景颜色。以下代码会把页面背景设置成草绿色：

```css
body{
	background-color: #bada55;
}
```

也可以直接使用更短的background属性:

```css
body{
	background: #bada55;
}
```



**颜色与不透明度**

在这个例子里，使用了十六进制表示法指定了颜色。所谓十六进制表示法，就是一个#后面加上6位十六进制数字构成的字符串。



3组数字分别表示颜色中的红、绿、蓝（RGB）通道的值。每种颜色通道的值有256种可能。如果3组数字中每组的2位数字相同，可以简写成3位数字，比如#aabbcc可以简写成#abc。



RGB值可以用另一种方式表示，即rgb()函数式表示法。RGB的每个值可以是一个十进制数值，取值范围是0-255；也可以是一个百分比值，取值范围是0-100%。用rgb()表示法表示前面例子中的背景颜色，结果如下：

```css
body{
	background-color: rgb(186,218,85);
}
```



接下来要介绍的颜色表示法是RGB的加强版，叫rgba()。其中末尾的a表示alpha，是用于控制透明度的阿尔法通道。如果想设置同样背景颜色的背景，但透明度为50%，可以这样做：

```css
.box{
	background-color: rgba(186,218,85,0.5);
}
```

第四个参数值表示透明度，取值范围为0-1.0，1.0表示完全不透明，0表示完全透明。





# 背景图片

添加背景颜色可以让页面色彩更加丰富。但有时候也想使用图片作为元素的背景。CSS为实现这些目标提供了充足的工具。



## 简单的背景图片示例

假设设计的页面要像twitter或facebook等社交网站的用户首页一样，需要显示各式各样的页头。



我们的页面是一个猫咪社交网站，如下图所示，需要包含一个大幅页头图和带个人信息的头像。

![CatBGImg](https://pvnk1u.github.io/images/CatBGImg.PNG)



首先设置一个灰蓝色的默认背景颜色，再添加一个背景图片。**添加默认背景颜色很重要，以防图片加载失败**：

```css
.profile-box{
	width:100%;
	height: 600px;
	background-color: #8Da9cf;
	background-image: url(img/big-cat.jpg)
}
```

组件的HTML代码如下：

```html
<header class="profile-box">
</header>
```

结果如图所示，图片在整个元素盒子范围内平铺，呈拼贴状：

![CatBGImg1](https://pvnk1u.github.io/images/CatBGImg1.PNG)



为什么图片会平铺到整个元素盒子？这是由背景图片的另一个相关属性background-repeat的默认值决定的。background-repeat属性的默认值为repeat，意思是背景图片要沿x轴和y轴重复。这个特性对花纹图案的背景图片非常有用，但对照片可能就不合适了。可以明确声明repeat-x和repeat-y来限定图片只沿某个轴的方向重复，但在此要通过no-repeat完全禁止重复：

```css
.profile-box{
	width:100%;
	height: 600px;
	background-color: #8Da9cf;
	background-image: url(img/big-cat.jpg);
	background-repeat: no-repeat;
}
```

最新的规范重新定义了这个属性，扩展了语法。支持以空格分隔的针对两个方向的关键词声明语法。换句话说，以下声明等同于使用repeat-x：

```css
.profile-box{
	background-repeat: repeat no-repeat;
}
```





# 背景图片语法

回到上面的例子，开始使用JPEG格式的图片来创建个人首页的示例。目前，图片已经平铺到元素的背景上，但仍然需要调整。目前的效果如下所示：

![CatBGImg2](https://pvnk1u.github.io/images/CatBGImg2.PNG)

可以看到现在背景图片是靠左显示的，右侧有了大量的空白。为了解决这个问题，需要设置背景图片的位置。



## 背景位置

背景图片的位置由background-position属性控制。



```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Bigger image, positioned in middle</title>

  <style>
    body {
      padding: 0;
      margin: 0;
      font-family: "Helvetica Neue", Arial, sans-serif;
    }
    .profile-box {
      width: 100%;
      height: 600px;
      background-color: #8Da9cf;
      background-image: url(img/big-cat.jpg);
      background-position: 50% 50%;
      background-repeat: no-repeat;
    }
  </style>
</head>
<body>
  <header class="profile-box" role="banner">

  </header>
</body>
</html>
```

此时，图片居中覆盖了整个元素。



background-position属性既可以使用关键字，也可以使用像素、em或百分比。最简单的情况下，可以只给两个值：一个表示相对于左侧的偏移量，一个表示相对于顶部的偏移量。



## 背景裁剪与原点

默认情况下，背景图片是绘制到元素边框以内的。如果把背景图片定位到边框下方，而边框又被设置为半透明，那么图片边缘就会出现半透明的边框。



使用background-clip属性可以改变这个行为。这个属性的默认值为background-clip: border-box，将其改为padding-box就可以把图片裁剪到内边距盒子以内。而content-box值则会把图片位于内边距及其之外的部分裁剪掉。下面的例子展示了这三个值的区别：

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Background clip example</title>

  <style>
    body {
      padding: 2em;
      margin: 0;
      font-family: "Helvetica Neue", Arial, sans-serif;
      background-color: #fff;
    }
    /* this default box is the same as background-clip: border-box; */
    .profile-box {
      display: inline-block;
      height: 200px;
      width: 400px;
      background-position: 50% 50%;
      background-repeat: no-repeat;
      background-image: url(img/cat.jpg);
      border: 10px solid rgba(220, 220, 160, 0.5);
      padding: 10px;
    }
    .padding-box-clipped {
      background-clip: padding-box;
    }
    .content-box-clipped {
      background-clip: content-box;
    }
  </style>
</head>
<body>
  <div class="profile-box"></div>
  <div class="profile-box padding-box-clipped"></div>
  <div class="profile-box content-box-clipped"></div>
</body>
</html>
```

相应的效果如下：

![background-clip](https://pvnk1u.github.io/images/background-clip.PNG)





## 背景附着

背景会附着在指定元素的后面，如果滚动页面，那么背景也会随着元素移动而移动。可以通过background-attachment属性改变这种行为。如果想让示例中的大背景图在页面滚动时“粘”在页面上，可以使用如下代码：

```css
.profile-box{
	background-attachment: fixed;
}
```

完整代码如下：

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Profile page with fixed background</title>

  <style>
    /**
    * 1. "Artificial" height applied to body, just to trigger a scrollable are.
    *    and see the effects.
    */
    body {
      padding: 0;
      margin: 0;
      font-family: "Helvetica Neue", Arial, sans-serif;
      height: 4000px; /* [1] */
    }
    /**
    * 1. Note that the positioning of the background now is in relation to the
    *    viewport, so we can't position it in the vertical middle of the header.
    */
    .profile-box {
      position: relative;
      height: 600px;
      background-image: url(img/big-cat.jpg);
      background-position: 50% 0; /* [1] */
      background-repeat: no-repeat;
      background-attachment: fixed;
    }

    .profile-photo {
      width: 160px;
      min-height: 200px;
      position: absolute;
      bottom: -60px;
      left: 5%;
    }
    .profile-photo img {
      display: block;
      max-width: 100%;
      height: auto;
    }
    .username {
      font-size: 20px;
      color: #666;
    }
  </style>
</head>
<body>
  <header class="profile-box" role="banner">
    <div class="profile-photo">
      <img src="img/profile.jpg" alt="Charles the Cat">
      <h1 class="username">@CharlesTheCat</h1>
    </div>
  </header>
</body>
</html>
```

通过这种方式，可以实现随着页面滚动，头部的背景图片好像藏到了页面后面，很酷的效果。



## 背景大小

在上一节的例子中，使用了一张大图覆盖了整个元素。这样的话，如果是在小屏幕上，那么图片会被裁切掉。反之，如果屏幕特别大，那么元素边缘可能出现空白。要避免上述情况，不管页面怎么缩放，都让内容保持自己的宽高比，就要使用background-size属性。



给background-size明确指定一个值，可以重新设置图片大小，也可以让它随元素大小缩放而缩放。



如果还是那张大图，由于某种原因希望它显示的小一点，那么可以重新给它一个尺寸：

```css
.profile-box{
	background-size: 400px 240px;
}
```

要让图片随元素缩放而缩放，则必须使用百分比值。不过要注意，**百分比值并不是相对于图片固有大小，而是相对于容器大小。因此，简单地把图片宽度和高度都设置成百分比值，可能会因容器高度变化而导致图片变形。**



更好的做法是只给一个维度设置百分比值，另一个维度设置关键字值auto。比如，要是想让图片宽度始终保持为100%（即x轴，第一个值），同时保持自己固有的宽高比，可以这样写：

```css
.profile-box{
	background-box: 100% auto;
}
```



百分比值赋予了我们控制的灵活度，但也不是任何情况下都适用。有时候，我们会希望背景图片的任何一边都不要被切掉。还有一种情况比如前面简介页面的头部区域，我们希望图片始终都能完全覆盖元素。好在CSS为此也提供了一些关键字值。



首先，可以把背景大小设置为contain。这个值可以让浏览器尽可能保持图片最大化，同时不改变图片的宽高比。与前面的例子类似，但浏览器会自动决定哪一边使用auto值，哪一边使用100%。代码如下所示：

```css
.profile-box{
	background-size: contain;
}
```

在高而窄的元素中，方形背景最多100%宽，因此垂直方向会出现空白；而在较宽的元素中，背景最多100%高，因此水平方向会出现空白。



然后，第二个关键字是cover，意思是图片会缩放以保证覆盖元素的每一个像素，同时不会变形。这正是我们希望的个人简介页面的效果。使用这个关键字，在一个高而窄的元素中，元素高度会被填满，但图片左右两边会被切掉；而在一个较宽的元素中，元素宽度会被填满，但图片上下两边会被切掉。代码如下:

```css
.profile-box{
	background-size: cover;
}
```





# 边框和圆角

现代浏览器对边框都提供了一定的控制，包括插入图片和圆角，让边框不再是简单的矩形。



先简单回顾下边框属性。

1. 可以分别为盒子的各边设置边框，也可以一次性为四边设置边框。
2. 可以使用border-width一次性设置所有边框的宽度，也可以使用border-top-width这样的方位属性设置某条边框的宽度。除非明确指定box-sizing属性，否则边框宽度会影响盒子的尺寸。
3. 同理，可以使用border-color设置所有边框的颜色，也可以使用border-left-color这样的方式属性设置某条边框的颜色。
4. 边框的样式可以使用border-style（或border-right-style这样的方位属性）来设置，取值中最常用的是solid、dashed或dotted等关键字。
5. 最后，可以使用border简写属性来设置所有边框属性。具体来说，可以把所有边设置成相同的宽度、样式、颜色，比如border: 2px solid #000;



## 边框半径：圆角



给border-radius属性一个长度值，就可以一次性设置盒子四个角的半径。下面给简介页面添加一个头像，让包含头像的元素拥有圆角。首先是标记：

```
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Profile with border radius</title>

  <style>
    body {
      padding: 0;
      margin: 0;
      font-family: "Helvetica Neue", Arial, sans-serif;
      background-color: #ddd;
    }
    .profile-box {
      position: relative;
      height: 600px;
      background-image: url(img/big-cat.jpg);
      background-size: cover;
      background-position: 50% 30%;
      border-bottom: 1px solid #666;
    }

    .profile-photo {
      width: 160px;
      min-height: 200px;
      position: absolute;
      bottom: -60px;
      left: 5%;
      padding: .5em;
      -webkit-border-radius: .5em;
              border-radius: .5em;
      background-color: #fff;
      border: 1px solid #777;
	  border-radius: 0.5em;
    }
    .profile-photo img {
      display: block;
      max-width: 100%;
      height: auto;
    }
    .username {
      font-size: 20px;
      color: #666;
      margin: .35em 0 0;
    }
  </style>
</head>
<body>
  <header class="profile-box" role="banner">
    <div class="profile-photo">
      <img src="img/profile.jpg" alt="Charles the Cat">
      <h1 class="username">@CharlesTheCat</h1>
    </div>
  </header>
</body>
</html>
```

在这段代码里，首先设置了父header容器的定位为相对定位：

```css
.profile-box{
	position: relative;
	/* 其他代码 */
}
```

然后设置了头像元素.profile-photo的样式：

```css
.profile-photo {
      width: 160px;
      min-height: 200px;
      position: absolute;
      bottom: -60px;
      left: 5%;
      padding: .5em;
      -webkit-border-radius: .5em;
              border-radius: .5em;
      background-color: #fff;
      border: 1px solid #777;
	  border-radius: 0.5em;
}
```

**在这里，必须要先设置父header容器的定位为relative相对定位，子元素头像元素profile-photo的定位才会生效。这是因为：在父元素没有设置相对定位或绝对定位的情况下，子元素设置absolute绝对定位时是相对于根元素（即HTML元素）来定位的。当父元素设置了相对或绝对定位，元素会相对于离自己最近的设置了相对或绝对定位的父元素进行定位。**定位设置完成后，又通过border设置了头像元素边框的宽度及颜色，并通过border-radius设置边框样式为圆角。







