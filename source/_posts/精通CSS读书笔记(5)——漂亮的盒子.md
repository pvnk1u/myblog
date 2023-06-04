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



