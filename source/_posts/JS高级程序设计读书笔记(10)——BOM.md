---
title: "JS高级程序设计读书笔记(10)——BOM" 
date: 2023-04-16T13:26:39+08:00
draft: false
tags: ["web","javascript"]
categories:
  - "web"
  - "javascript"
toc: true
---



虽然 `ECMAScript` 把浏览器对象模型（`BOM`，`Browser Object Model`）描述为 `JavaScript` 的核心，但 实际上 `BOM` 是使用 `JavaScript` 开发 `Web` 应用程序的核心。`BOM` 提供了与网页无关的浏览器功能对象。 多年来，`BOM` 是在缺乏规范的背景下发展起来的，因此既充满乐趣又问题多多。毕竟，浏览器开发商 都按照自己的意愿来为它添砖加瓦。最终，浏览器实现之间共通的部分成为了事实标准，为 `Web` 开发 提供了浏览器间互操作的基础。`HTML5` 规范中有一部分涵盖了 `BOM` 的主要内容，因为 `W3C` 希望将 `JavaScript` 在浏览器中最基础的部分标准化。



# window对象

`BOM` 的核心是 `window` 对象，表示浏览器的实例。`window` 对象在浏览器中有两重身份，一个是 `ECMAScript` 中的 `Global` 对象，另一个就是浏览器窗口的 `JavaScript` 接口。这意味着网页中定义的所有 对象、变量和函数都以 `window`作为其 `Global`对象，都可以访问其上定义的 `parseInt()`等全局方法。



## Global作用域

因为 `window` 对象被复用为 `ECMAScript`的 `Global` 对象，所以通过 `var` 声明的所有全局变量和函数都会变成 `window` 对象的属性和方法。比如：

```js
var age = 29;
var sayAge = () => alert(this.age);

alert(window.age); // 29
sayAge(); // 29
window.sayAge(); // 29
```

这里，变量 `age` 和函数 `sayAge()`被定义在全局作用域中，它们自动成为了 `window` 对象的成员。 因此，变量 `age` 可以通过 `window.age` 来访问，而函数 `sayAge()`也可以通过 `window.sayAge()`来访问。 因为 `sayAge()`存在于全局作用域，`this.age` 映射到 `window.age`，所以就可以显示正确的结果了。



如果在这里使用 `let` 或 `const` 替代 `var`，则不会把变量添加给全局对象：

```js
let age = 29;
const sayAge = () => alert(this.age);

alert(window.age); // undefined
sayAge(); // undefined
window.sayAge(); // TypeError: window.sayAge is not a function
```

另外，访问未声明的变量会抛出错误，但是可以在 `window` 对象上查询是否存在可能未声明的变量。 比如：

```js
// 这会导致抛出错误，因为 oldValue 没有声明
var newValue = oldValue;
// 这不会抛出错误，因为这里是属性查询
// newValue 会被设置为 undefined
var newValue = window.oldValue;
```

`JavaScript` 中有很多对象都暴露在全局作用域中，比如 `location` 和 `navigator`，因而它们也是 `window` 对象的属性。



## 窗口关系

`top` 对象始终指向最上层（最外层）窗口，即浏览器窗口本身。而 `parent` 对象则始终指向当前窗口的父窗口。如果当前窗口是最上层窗口，则 `parent` 等于 `top`（都等于 `window`）。最上层的 `window` 如果不是通过 `window.open()`打开的，那么其 `name` 属性就不会包含值。



还有一个 `self` 对象，它是终极 `window` 属性，始终会指向 `window`。实际上，`self` 和 `window` 就 是同一个对象。之所以还要暴露 `self`，就是为了和 `top`、`parent` 保持一致。



这些属性都是 `window` 对象的属性，因此访问 `window.parent`、`window.top` 和 `window.self` 都可以。这意味着可以把访问多个窗口的 `window` 对象串联起来，比如 `window.parent.parent`。



## 窗口位置与像素比

`window` 对象的位置可以通过不同的属性和方法来确定。现代浏览器提供了 `screenLeft` 和 `screenTop` 属性，用于表示窗口相对于屏幕左侧和顶部的位置 ，返回值的单位是 `CSS` 像素。

可以使用 `moveTo()`和 `moveBy()`方法移动窗口。这两个方法都接收两个参数，其中 `moveTo()`接收要移动到的新位置的绝对坐标 x 和 y；而 `moveBy()`则接收相对当前位置在两个方向上移动的像素数。 比如：

```js
// 把窗口移动到左上角
window.moveTo(0,0);

// 把窗口向下移动 100 像素
window.moveBy(0, 100); 

// 把窗口移动到坐标位置(200, 300)
window.moveTo(200, 300);

// 把窗口向左移动 50 像素
window.moveBy(-50, 0); 
```



## 窗口大小

在不同浏览器中确定浏览器窗口大小没有想象中那么容易。所有现代浏览器都支持 4 个属性： `innerWidth`、`innerHeight`、`outerWidth` 和 `outerHeight`。`outerWidth` 和 `outerHeight` 返回浏 览器窗口自身的大小（不管是在最外层 `window` 上使用，还是在窗格中使用）。`innerWidth` 和 `innerHeight` 返回浏览器窗口中页面视口的大小（不包含浏览器边框和工具栏）。



`document.documentElement.clientWidth` 和 `document.documentElement.clientHeight` 返回页面视口的宽度和高度。



## 定时器

`JavaScript` 在浏览器中是单线程执行的，但允许使用定时器指定在某个时间之后或每隔一段时间就 执行相应的代码。`setTimeout()`用于指定在一定时间后执行某些代码，而 `setInterval()`用于指定 每隔一段时间执行某些代码。



`setTimeout()`方法通常接收两个参数：要执行的代码和在执行回调函数前等待的时间（毫秒）。第 一个参数可以是包含 `JavaScript` 代码的字符串（类似于传给 `eval()`的字符串）或者一个函数，比如：

```js
// 在 1 秒后显示警告框
setTimeout(() => alert("Hello world!"), 1000);
```



第二个参数是要等待的毫秒数，而不是要执行代码的确切时间。`JavaScript` 是单线程的，所以每次 只能执行一段代码。为了调度不同代码的执行，`JavaScript` 维护了一个任务队列。其中的任务会按照添 加到队列的先后顺序执行。`setTimeout()`的第二个参数只是告诉 `JavaScript` 引擎在指定的毫秒数过后 把任务添加到这个队列。**如果队列是空的，则会立即执行该代码。如果队列不是空的，则代码必须等待 前面的任务执行完才能执行。**



调用 `setTimeout()`时，会返回一个表示该超时排期的数值 ID。这个超时 ID 是被排期执行代码的唯一标识符，可用于取消该任务。要取消等待中的排期任务，可以调用 `clearTimeout()`方法并传入超 时 ID，如下面的例子所示：

```js
// 设置超时任务
let timeoutId = setTimeout(() => alert("Hello world!"), 1000);

// 取消超时任务
clearTimeout(timeoutId);
```

只要是在指定时间到达之前调用 `clearTimeout()`，就可以取消超时任务。在任务执行后再调用 `clearTimeout()`没有效果。



`setInterval()`与 `setTimeout()`的使用方法类似，只不过指定的任务会每隔指定时间就执行一次，直到取消循环定时或者页面卸载。`setInterval()`同样可以接收两个参数：要执行的代码（字符 串或函数），以及把下一次执行定时代码的任务添加到队列要等待的时间（毫秒）。下面是一个例子：

```js
setInterval(() => alert("Hello world!"), 10000);
```

这里的关键点是，第二个参数，也就是间隔时间，指的是向队列添加新任务之前等 待的时间。比如，调用 `setInterval()`的时间为 `01:00:00`，间隔时间为 3000 毫秒。这意 味着 `01:00:03` 时，浏览器会把任务添加到执行队列。浏览器不关心这个任务什么时候执行 或者执行要花多长时间。因此，到了 `01:00:06`，它会再向队列中添加一个任务。由此可看 出，执行时间短、非阻塞的回调函数比较适合 `setInterval()`。



`setInterval()`方法也会返回一个循环定时 ID，可以用于在未来某个时间点上取消循环定时。要 取消循环定时，可以调用 `clearInterval()`并传入定时 ID。相对于 `setTimeout()`而言，取消定时的能力对 `setInterval()`更加重要。毕竟，如果一直不管它，那么定时任务会一直执行到页面卸载。



`setIntervale()`在实践中很少会在 生产环境下使用，因为一个任务结束和下一个任务开始之间的时间间隔是无法保证的，有些循环定时任务可能会因此而被跳过。而像前面这个例子中一样使用 `setTimeout()`则能确保不会出现这种情况。一 般来说，最好不要使用 `setInterval()`。





## 系统对话框

使用 `alert()`、`confirm()`和 `prompt()`方法，可以让浏览器调用系统对话框向用户显示消息。这 些对话框与浏览器中显示的网页无关，而且也不包含 `HTML`。它们的外观由操作系统或者浏览器决定， 无法使用 `CSS` 设置。此外，这些对话框都是同步的模态对话框，即在它们显示的时候，代码会停止执行， 在它们消失以后，代码才会恢复执行。



确认框，通过调用 `confirm()`来显示。确认框跟警告框类似，都会向用户显示消 息。但不同之处在于，确认框有两个按钮：“Cancel”（取消）和“OK”（确定）。用户通过单击不同的按 钮表明希望接下来执行什么操作。要知道用户单击了 OK 按钮还是 Cancel 按钮，可以判断 confirm()方法的返回值：true 表示单击 了 OK 按钮，false 表示单击了 Cancel 按钮或者通过单击某一角上的 X 图标关闭了确认框。


最后一种对话框是提示框，通过调用 `prompt()`方法来显示。提示框的用途是提示用户输入消息。 除了 OK 和 Cancel 按钮，提示框还会显示一个文本框，让用户输入内容。`prompt()`方法接收两个参数： 要显示给用户的文本，以及文本框的默认值（可以是空字符串）。



如果用户单击了 OK 按钮，则 `prompt()`会返回文本框中的值。如果用户单击了 `Cancel` 按钮，或者 对话框被关闭，则 `prompt()`会返回 `null`。



# location对象

`location` 是最有用的 `BOM` 对象之一，提供了当前窗口中加载文档的信息，以及通常的导航功能。 这个对象独特的地方在于，它既是 `window` 的属性，也是 `document` 的属性。也就是说， `window.location` 和 `document.location` 指向同一个对象。`location` 对象不仅保存着当前加载文 档的信息，也保存着把 `URL` 解析为离散片段后能够通过属性访问的信息。



假设浏览器当前加载的 URL 是 `http://foouser:barpassword@www.wrox.com:80/WileyCDA/?q=javascript#contents`，`location` 对象的内容如下表所示。

| 属性              | 值                                                        | 说明                                                         |
| ----------------- | --------------------------------------------------------- | ------------------------------------------------------------ |
| location.hash     | "#contents"                                               | URL 散列值（井号后跟零或多个字符），如果没有则 为空字符串    |
| location.host     | "www.wrox.com:80"                                         | 服务器名及端口号                                             |
| location.hostname | "www.wrox.com"                                            | 服务器名                                                     |
| location.href     | "http://www.wrox.com:80/WileyCDA/ ?q=javascript#contents" | 当前加载页面的完整 URL。location 的 toString() 方法返回这个值 |
| location.pathname | "/WileyCDA/"                                              | URL 中的路径和（或）文件名                                   |
| location.port     | "80                                                       | 请求的端口。如果 URL中没有端口，则返回空字符串               |
| location.protocol | "http:"                                                   | 页面使用的协议。通常是"http:"或"https:"                      |
| location.search   | "?q=javascript"                                           | URL 的查询字符串。这个字符串以问号开头                       |
| location.username | "foouser"                                                 | 域名前指定的用户名                                           |
| location.password | "barpassword"                                             | 域名前指定的密码                                             |
| location.origin   | "http://www.wrox.com"                                     | URL 的源地址。只读                                           |



