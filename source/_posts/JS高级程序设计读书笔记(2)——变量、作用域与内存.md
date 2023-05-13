---
title: "JS高级程序设计读书笔记(2)——变量、作用域与内存" 
date: 2022-08-16T12:32:39+08:00
draft: false
tags: ["web","javascript"]
categories:
  - "web"
  - "javascript"
toc: true
---



相比于其他语言，`JavaScript`中的变量可谓独树一帜。由于没有规则定义变量必须包含什么数据类型，变量的值和数据类型在脚本生命期内可以改变。这样的变量很有意思，很强大，当然也有不少问题。

# 原始值与引用值

`ECMAScript` 变量可以包含两种不同类型的数据：原始值和引用值。原始值（`primitive  value`）就是最简单的数据，引用值（`reference value`）则是由多个值构成的对象。



在把一个值赋给变量时，`JavaScript` 引擎必须确定这个值是原始值还是引用值。上一章讨论了 6 种原始值：`Undefined`、`Null`、`Boolean`、`Number`、`String `和`Symbol`。保存原始值的变量是按值（`by value`）访问的，因为我们操作的就是存储在变量中的实际值。



引用值是保存在内存中的对象。与其他语言不同，`JavaScript` 不允许直接访问内存位置，因此也就不能直接操作对象所在的内存空间。在操作对象时，实际上操作的是对该对象的引用（`reference`）而非实际的对象本身。为此，保存引用值的变量是按引用（`by reference`）访问的。



在很多语言中，字符串是使用对象表示的，因此被认为是引用类型。`ECMAScript`打破了这个惯例。



## 动态属性

原始值和引用值的定义方式很类似，都是创建一个变量，然后给它赋一个值。不过，在变量保存了这个值之后，可以对这个值做什么，则大有不同。对于引用值而言，可以随时添加、修改和删除其属性和方法。比如，看下面的例子：

```js
let person = new Object(); 
person.name = "Nicholas"; 
console.log(person.name); // "Nicholas"
```

这里，首先创建了一个对象，并把它保存在变量 person 中。然后，给这个对象添加了一个名为name 的属性，并给这个属性赋值了一个字符串"Nicholas"。在此之后，就可以访问这个新属性，直到对象被销毁或属性被显式地删除。在此之后，就可以访问这个新属性，直到对象被销毁或属性被显式地删除。



原始值不能有属性，尽管尝试给原始值添加属性不会报错。比如：

```js
let name = "Nicholas"; 
name.age = 27; 
console.log(name.age);  // undefined
```

只有引用值可以动态添加后面可以使用的属性。



**原始类型的初始化可以只使用原始字面量形式**。如果使用的是new 关键字，则`JavaScript` 会创建一个`Object` 类型的实例，但其行为类似原始值。下面来看看这两种初始化方式的差异：

```js
let name1 = "Nicholas"; 

let name2 = new String("Matt"); 
name1.age = 27; 
name2.age = 26; 
console.log(name1.age);    // undefined 

console.log(name2.age);    // 26 
console.log(typeof name1); // string 
console.log(typeof name2); // object
```



## 复制值

除了存储方式不同，原始值和引用值在通过变量复制时也有所不同。在通过变量把一个原始值赋值到另一个变量时，原始值会被复制到新变量的位置：

```js
let num1 = 5; 
let num2 = num1;
```

这里，`num1` 包含数值5。当把`num2` 初始化为`num1` 时，`num2` 也会得到数值5。这个值跟存储在`num1` 中的5 是完全独立的，因为它是那个值的副本。



在把引用值从一个变量赋给另一个变量时，存储在变量中的值也会被复制到新变量所在的位置。区别在于，这里复制的值实际上是一个指针，它指向存储在堆内存中的对象。操作完成后，两个变量实际上指向同一个对象，因此一个对象上面的变化会在另一个对象上反映出来，如下面的例子所示：

```js
let obj1 = new Object(); 
let obj2 = obj1; 
obj1.name = "Nicholas"; 
console.log(obj2.name); // "Nicholas"
```



## 传递参数

`ECMAScript` 中**所有函数的参数都是按值传递的**。这意味着函数外的值会被复制到函数内部的参数中，就像从一个变量复制到另一个变量一样。如果是原始值，那么就跟原始值变量的复制一样，如果是引用值，那么就跟引用值变量的复制一样。对很多开发者来说，这一块可能会不好理解，毕竟变量有按值和按引用访问，而传参则只有按值传递。



在按值传递参数时，值会被复制到一个局部变量。在按引用传递参数时，值在内存中的位置会被保存在一个局部变量，这意味着对本地变量的修改会反映到函数外部。（这在`ECMAScript` 中是不可能的。）来看下面这个例子：

```js
function addTen(num) { 
  num += 10;  
  return num; 
} 
 

let count = 20;
let result = addTen(count); 
console.log(count);  // 20，没有变化 

console.log(result); // 30 
```



这里，函数`addTen()`有一个参数`num`，它其实是一个局部变量。在调用时，变量`count` 作为参数传入。`count` 的值是20，这个值被复制到参数`num` 以便在`addTen()`内部使用。在函数内部，参数`num`的值被加上了10，但这不会影响函数外部的原始变量count。参数 `num` 和变量`count` 互不干扰，它们只不过碰巧保存了一样的值。如果`num` 是按引用传递的，那么`count` 的值也会被修改为30。这个事实在使用数值这样的原始值时是非常明显的。但是，如果变量中传递的是对象，就没那么清楚了。比如，再看这个例子：

```js
function setName(obj) { 
  obj.name = "Nicholas";  
} 
 

let person = new Object(); 
setName(person); 
console.log(person.name);  // "Nicholas"
```



这一次，我们创建了一个对象并把它保存在变量 `person` 中。然后，这个对象被传给 `setName()`方法，并被复制到参数`obj` 中。在函数内部，`obj` 和`person` 都指向同一个对象。结果就是，即使对象是按值传进函数的，`obj` 也会通过引用访问对象。当函数内部给`obj` 设置了`name`属性时，函数外部的对象也会反映这个变化，因为`obj` 指向的对象保存在全局作用域的堆内存上。很多开发者错误地认为，当在局部作用域中修改对象而变化反映到全局时，就意味着参数是按引用传递的。为证明对象是按值传递的，我们再来看看下面这个修改后的例子：

```js
function setName(obj) { 
  obj.name = "Nicholas";  
  obj = new Object(); 
  obj.name = "Greg"; 
} 

 
let person = new Object(); 
setName(person); 

console.log(person.name);  // "Nicholas" 
```

这个例子前后唯一的变化就是`setName()`中多了两行代码，将`obj` 重新定义为一个有着不同name的新对象。当`person` 传入`setName()`时，其`name` 属性被设置为"Nicholas"。然后变量`obj` 被设置为一个新对象且`name` 属性被设置为"Greg"。如果 `person` 是按引用传递的，那么`person` 应该自动将指针改为指向`name `为"Greg"的对象。可是，当我们再次访问`person.name` 时，它的值是"Nicholas"，这表明函数中参数的值改变之后，原始的引用仍然没变。当`obj `在函数内部被重写时，它变成了一个指向本地对象的指针。而那个本地对象在函数执行结束时就被销毁了。

## 确定类型

`typeof` 操作符最适合用来判断一个变量是否为原始类型。更确切地说，它是判断一个变量是否为字符串、数值、布尔值或`undefined` 的最好方式。如果值是对象或`null`，那么`typeof`返回"object":

```js
let s = "Nicholas"; 
let b = true; 
let i = 22; 

let u; 
let n = null; 
let o = new Object(); 

console.log(typeof s); // string 
console.log(typeof i); // number 
console.log(typeof b); // boolean 

console.log(typeof u); // undefined 
console.log(typeof n); // object 
console.log(typeof o); // object
```

`typeof `虽然对原始值很有用，但它对引用值的用处不大。我们通常不关心一个值是不是对象，而是想知道它是什么类型的对象。为了解决这个问题，`ECMAScript` 提供了`instanceof` 操作符，语法如下：

```js
result = variable instanceof constructor
```

如果变量是给定引用类型（由其原型链决定）的实例，则`instanceof` 操作符返回`true`。来看下面的例子：

```js
console.log(person instanceof Object);  // 变量person 是Object 吗？ 
console.log(colors instanceof Array);   // 变量colors 是Array 吗？ 
console.log(pattern instanceof RegExp); // 变量pattern 是RegExp 吗？
```

按照定义，所有引用值都是 `Object` 的实例，因此通过 `instanceof` 操作符检测任何引用值和`Object` 构造函数都会返回`true`。类似地，如果用`instanceof` 检测原始值，则始终会返回`false`，因为原始值不是对象。



# 执行上下文与作用域

执行上下文（以下简称“上下文”）的概念在`JavaScript` 中是颇为重要的。变量或函数的上下文决定了它们可以访问哪些数据，以及它们的行为。每个上下文都有一个关联的变量对象（`variable  object`），而这个上下文中定义的所有变量和函数都存在于这个对象上。虽然无法通过代码访问变量对象，但后台处理数据会用到它。 



全局上下文是最外层的上下文。根据`ECMAScript` 实现的宿主环境，表示全局上下文的对象可能不一样。在浏览器中，全局上下文就是常说的`window` 对象，因此所有通过`var` 定义的全局变量和函数都会成为`window` 对象的属性和方法。使用`let` 和`const` 的顶级声明不会定义在全局上下文中，但在作用域链解析上效果是一样的。上下文在其所有代码都执行完毕后会被销毁，包括定义在它上面的所有变量和函数（全局上下文在应用程序退出前才会被销毁，比如关闭网页或退出浏览器）。



每个函数调用都有自己的上下文。当代码执行流进入函数时，函数的上下文被推到一个上下文栈上。在函数执行完之后，上下文栈会弹出该函数上下文，将控制权返还给之前的执行上下文。`ECMAScript`程序的执行流就是通过这个上下文栈进行控制的。



上下文中的代码在执行的时候，会创建变量对象的一个作用域链（`scope chain`）。这个作用域链决定了各级上下文中的代码在访问变量和函数时的顺序。代码正在执行的上下文的变量对象始终位于作用域链的最前端。如果上下文是函数，则其活动对象（`activation  object`）用作变量对象。活动对象最初只有一个定义变量：`arguments`。（全局上下文中没有这个变量。）作用域链中的下一个变量对象来自包含上下文，再下一个对象来自再下一个包含上下文。以此类推直至全局上下文；全局上下文的变量对象始终是作用域链的最后一个变量对象。



代码执行时的标识符解析是通过沿作用域链逐级搜索标识符名称完成的。搜索过程始终从作用域链的最前端开始，然后逐级往后，直到找到标识符。（如果没有找到标识符，那么通常会报错。）



看一看下面这个例子：

```js
var color = "blue";  
 
function changeColor() { 

  if (color === "blue") { 
    color = "red"; 
  } else { 

    color = "blue"; 
  } 
} 

 

changeColor();
```

对这个例子而言，函数 `changeColor()`的作用域链包含两个对象：一个是它自己的变量对象（就是定义`arguments` 对象的那个），另一个是全局上下文的变量对象。这个函数内部之所以能够访问变量`color`，就是因为可以在作用域链中找到它。



此外，局部作用域中定义的变量可用于在局部上下文中替换全局变量。看一看下面这个例子：

```js
var color = "blue";  

 
function changeColor() { 
  let anotherColor = "red"; 

 
  function swapColors() { 
    let tempColor = anotherColor; 

    anotherColor = color; 
    color = tempColor; 
    // 这里可以访问color、anotherColor 和tempColor 
  } 

  // 这里可以访问color 和anotherColor，但访问不到tempColor 
  swapColors(); 

} 
 
// 这里只能访问color 
changeColor();
```



## 作用域链增强

虽然执行上下文主要有`全局上下文`和`函数上下文`两种（`eval()`调用内部存在第三种上下文），但有其他方式来增强作用域链。某些语句会导致在作用域链前端临时添加一个上下文，这个上下文在代码执行后会被删除。通常在两种情况下会出现这个现象，即代码执行到下面任意一种情况时：

1. try/catch 语句的catch 块
2. with 语句



这两种情况下，都会在作用域链前端添加一个变量对象。对`with` 语句来说，会向作用域链前端添加指定的对象；对 `catch` 语句而言，则会创建一个新的变量对象，这个变量对象会包含要抛出的错误对象的声明。看下面的例子：

```js
function buildUrl() {  
  let qs = "?debug=true"; 
 
  with(location){ 
    let url = href + qs; 

  } 
 
  return url; 
}
```

这里，`with` 语句将 `location` 对象作为上下文，因此 location 会被添加到作用域链前端。`buildUrl()`函数中定义了一个变量 `qs`。当 `with` 语句中的代码引用变量 `href` 时，实际上引用的是`location.href`，也就是自己变量对象的属性。在引用`qs` 时，引用的则是定义在`buildUrl()`中的那个变量，它定义在函数上下文的变量对象上。而在 `with` 语句中使用 `var` 声明的变量 `url` 会成为函数上下文的一部分，可以作为函数的值被返回；但像这里使用`let` 声明的变量`url`，因为被限制在块级作用域（稍后介绍），所以在`with` 块之外没有定义。

## 变量声明

`ES6` 之后，`JavaScript` 的变量声明经历了翻天覆地的变化。直到`ECMAScript 5.1`，`var` 都是声明变量的唯一关键字。`ES6` 不仅增加了 `let` 和 `const` 两个关键字，而且还让这两个关键字压倒性地超越 `var`成为首选。

1. 使用var 的函数作用域声明

   在使用`var` 声明变量时，**变量会被自动添加到最接近的上下文**。在函数中，最接近的上下文就是函数的局部上下文。在`with` 语句中，最接近的上下文也是函数上下文。**如果变量未经声明就被初始化了，那么它就会自动被添加到全局上下文**，如下面的例子所示： 

   ```js
   function add(num1, num2) { 
     var sum = num1 + num2;  
     return sum; 
   } 
    
   let result = add(10, 20); // 30 
   console.log(sum);         // 报错：sum 在这里不是有效变量
   ```

   

   这里，函数`add()`定义了一个局部变量`sum`，保存加法操作的结果。这个值作为函数的值被返回，但变量`sum` 在函数外部是访问不到的。如果省略上面例子中的关键字`var`，那么`sum` 在`add()`被调用之后就变成可以访问的了，如下所示：

   ```js
   function add(num1, num2) { 
     sum = num1 + num2; 
     return sum;  
   
   } 
    
   let result = add(10, 20); // 30 
   console.log(sum);         // 30
   ```

   这一次，变量 `sum` 被用加法操作的结果初始化时并没有使用 `var` 声明。在调用 `add()`之后，`sum`被添加到了全局上下文，在函数退出之后依然存在，从而在后面可以访问到。

   未经声明而初始化变量是`JavaScript` 编程中一个非常常见的错误，会导致很多问题。为此，在初始化变量之前一定要先声明变量。在严格模式下，未经声明就初始化变量会报错。

2. 使用let 的块级作用域声明

   `ES6` 新增的`let` 关键字跟`var` 很相似，但**它的作用域是块级的**，这也是`JavaScript` 中的新概念。**块级作用域由最近的一对包含花括号`{}`界定**。换句话说，`if` 块、`while` 块、`function` 块，甚至连单独的块也是`let` 声明变量的作用域。

   ```js
   if (true) { 
     let a;  
   } 
   
   console.log(a); // ReferenceError: a 没有定义 
   
   while (true) { 
     let b; 
   }
   
   console.log(b); // ReferenceError: b 没有定义 
    
   function foo() { 
     let c; 
   } 
   
   console.log(c); // ReferenceError: c 没有定义 
                   // 这没什么可奇怪的 
                   // var 声明也会导致报错 
    
   // 这不是对象字面量，而是一个独立的块 
   // JavaScript 解释器会根据其中内容识别出它来 
   { 
     let d; 
   } 
   console.log(d); // ReferenceError: d 没有定义
   ```

   `let` 与 `var`的另一个不同之处是在同一作用域内不能声明两次。重复的 `var` 声明会被忽略，而重复的`let` 声明会抛出`SyntaxError`。

   ```js
   var a; 
   var a; 
   // 不会报错 
    
   
   {  
     let b; 
     let b; 
   
   } 
   
   // SyntaxError: 标识符b 已经声明过了
   ```

   **`let` 的行为非常适合在循环中声明迭代变量。使用`var` 声明的迭代变量会泄漏到循环外部，这种情况应该避免**。来看下面两个例子：

   ```js
   for (var i = 0; i < 10; ++i) {} 
   console.log(i); // 10 
    
   
   for (let j = 0; j < 10; ++j) {} 
   
   console.log(j); // ReferenceError: j 没有定义
   ```

3. 使用const 的常量声明

   除了`let`，`ES6` 同时还增加了`const` 关键字。使用`const` 声明的变量必须同时初始化为某个值。一经声明，在其生命周期的任何时候都不能再重新赋予新值。 

   ```js
   const a; // SyntaxError: 常量声明时没有初始化 
    
   const b = 3; 
   console.log(b); // 3 
   b = 4; // TypeError: 给常量赋值
   ```

   `const` 除了要遵循以上规则，其他方面与`let` 声明是一样的。

   `const` 声明只应用到顶级原语或者对象。换句话说，赋值为对象的 `const` 变量不能再被重新赋值为其他引用值，但对象的键则不受限制。

   ```js
   const o1 = {}; 
   o1 = {}; // TypeError: 给常量赋值 
    
   const o2 = {}; 
   o2.name = 'Jake'; 
   console.log(o2.name); // 'Jake'
   ```

   如果想让整个对象都不能修改，可以使用`Object.freeze()`，这样再给属性赋值时虽然不会报错，但会静默失败：

   ```js
   const o3 = Object.freeze({}); 
   o3.name = 'Jake'; 
   
   console.log(o3.name); // undefined
   ```

4. 标识符查找

   当在特定上下文中为读取或写入而引用一个标识符时，必须通过搜索确定这个标识符表示什么。搜索开始于作用域链前端，以给定的名称搜索对应的标识符。如果在局部上下文中找到该标识符，则搜索停止，变量确定；如果没有找到变量名，则继续沿作用域链搜索。（注意，作用域链中的对象也有一个原型链，因此搜索可能涉及每个对象的原型链。）这个过程一直持续到搜索至全局上下文的变量对象。如果仍然没有找到标识符，则说明其未声明。

   一个例子：

   ```js
   var color = 'blue'; 
    
   function getColor() { 
     return color;  
   } 
   
   console.log(getColor()); // 'blue'
   ```

   在这个例子中，调用函数`getColor()`时会引用变量`color`。为确定`color` 的值会进行两步搜索。第一步，搜索 `getColor()`的变量对象，查找名为 `color` 的标识符。结果没找到，于是继续搜索下一个变量对象（来自全局上下文），然后就找到了名为 `color` 的标识符。因为全局变量对象上有 `color`的定义，所以搜索结束。

   对这个搜索过程而言，引用局部变量会让搜索自动停止，而不继续搜索下一级变量对象。也就是说，如果局部上下文中有一个同名的标识符，那就不能在该上下文中引用父上下文中的同名标识符，如下面的例子所示：

   ```js
   var color = 'blue';  
    
   function getColor() { 
     let color = 'red'; 
     return color; 
   
   } 
    
   console.log(getColor()); // 'red'
   ```

   使用块级作用域声明并不会改变搜索流程，但可以给词法层级添加额外的层次：

   ```js
   var color = 'blue'; 
    
   function getColor() {  
     let color = 'red'; 
     { 
   
       let color = 'green'; 
       return color; 
     } 
   
   } 
    
   console.log(getColor()); // 'green' 
   ```

   在这个修改后的例子中，`getColor()`内部声明了一个名为`color` 的局部变量。在调用这个函数时，变量会被声明。在执行到函数返回语句时，代码引用了变量`color`。于是开始在局部上下文中搜索这个标识符，结果找到了值为'green'的变量`color`。因为变量已找到，搜索随即停止，所以就使用这个局部变量。这意味着函数会返回'green'。在局部变量 `color` 声明之后的任何代码都无法访问全局变量`color`，除非使用完全限定的写法`window.color`。



# 垃圾回收

`JavaScript `是使用垃圾回收的语言，也就是说执行环境负责在代码执行时管理内存。在 C 和 C++等语言中，跟踪内存使用对开发者来说是个很大的负担，也是很多问题的来源。`JavaScript` 为开发者卸下了这个负担，通过自动内存管理实现内存分配和闲置资源回收。基本思路很简单：确定哪个变量不会再使用，然后释放它占用的内存。这个过程是周期性的，即垃圾回收程序每隔一定时间（或者说在代码执行过程中某个预定的收集时间）就会自动运行。



以函数中局部变量的正常生命周期为例。函数中的局部变量会在函数执行时存在。此时，栈（或堆）内存会分配空间以保存相应的值。函数在内部使用了变量，然后退出。此时，就不再需要那个局部变量了，它占用的内存可以释放，供后面使用。这种情况下显然不再需要局部变量了，但并不是所有时候都会这么明显。垃圾回收程序必须跟踪记录哪个变量还会使用，以及哪个变量不会再使用，以便回收内存。如何标记未使用的变量也许有不同的实现方式。不过，在浏览器的发展史上，用到过两种主要的标记策略：标记清理和引用计数。



## 标记清理

`JavaScript` 最常用的垃圾回收策略是标记清理（`mark-and-sweep`）。当变量进入上下文，比如在函数内部声明一个变量时，这个变量会被加上存在于上下文中的标记。而在上下文中的变量，逻辑上讲，永远不应该释放它们的内存，因为只要上下文中的代码在运行，就有可能用到它们。当变量离开上下文时，也会被加上离开上下文的标记。



垃圾回收程序运行的时候，会标记内存中存储的所有变量（记住，标记方法有很多种）。然后，它会将所有在上下文中的变量，以及被在上下文中的变量引用的变量的标记去掉。在此之后再被加上标记的变量就是待删除的了，原因是任何在上下文中的变量都访问不到它们了。随后垃圾回收程序做一次内存清理，销毁带标记的所有值并收回它们的内存。



## 引用计数

另一种没那么常用的垃圾回收策略是引用计数（`reference counting`）。其思路是对每个值都记录它被引用的次数。声明变量并给它赋一个引用值时，这个值的引用数为1。如果同一个值又被赋给另一个变量，那么引用数加 1。类似地，如果保存对该值引用的变量被其他值给覆盖了，那么引用数减 1。当一个值的引用数为0 时，就说明没办法再访问到这个值了，因此可以安全地收回其内存了。垃圾回收程序下次运行的时候就会释放引用数为0 的值的内存。



引用计数最早由`Netscape Navigator` 3.0 采用，但很快就遇到了严重的问题：循环引用。所谓循环引用，就是对象A 有一个指针指向对象B，而对象B 也引用了对象A。比如：

```js
function problem() {  
  let objectA = new Object(); 
  let objectB = new Object(); 

 
  objectA.someOtherObject = objectB; 
  objectB.anotherObject = objectA; 
}
```

在这个例子中，`objectA` 和`objectB` 通过各自的属性相互引用，意味着它们的引用数都是2。在标记清理策略下，这不是问题，因为在函数结束后，这两个对象都不在作用域中。而在引用计数策略下，`objectA` 和`objectB` 在函数结束后还会存在，因为它们的引用数永远不会变成0。如果函数被多次调用，则会导致大量内存永远不会被释放。



## 性能

垃圾回收程序会周期性运行，如果内存中分配了很多变量，则可能造成性能损失，因此**垃圾回收的时间调度很重要**。尤其是在内存有限的移动设备上，垃圾回收有可能会明显拖慢渲染的速度和帧速率。**开发者不知道什么时候运行时会收集垃圾，因此最好的办法是在写代码时就要做到：无论什么时候开始收集垃圾，都能让它尽快结束工作**。



现代垃圾回收程序会基于对 `JavaScript` 运行时环境的探测来决定何时运行。探测机制因引擎而异，但基本上都是根据已分配对象的大小和数量来判断的。比如，根据 `V8` 团队 2016 年的一篇博文的说法：“在一次完整的垃圾回收之后，`V8` 的堆增长策略会根据活跃对象的数量外加一些余量来确定何时再次垃圾回收。”



## 内存管理

将内存占用量保持在一个较小的值可以让页面性能更好。优化内存占用的最佳手段就是保证在执行代码时只保存必要的数据。如果数据不再必要，那么把它设置为 `null`，从而释放其引用。这也可以叫作解除引用。这个建议最适合全局变量和全局对象的属性。局部变量在超出作用域后会被自动解除引用，如下面的例子所示：

```js
function createPerson(name){  
  let localPerson = new Object(); 
  localPerson.name = name; 
  return localPerson; 

} 
 
let globalPerson = createPerson("Nicholas"); 

 
// 解除globalPerson 对值的引用 
 
globalPerson = null; 
```

在上面的代码中，变量`globalPerson` 保存着`createPerson()`函数调用返回的值。在`createPerson()`内部，`localPerson` 创建了一个对象并给它添加了一个`name` 属性。然后，`localPerson` 作为函数值被返回，并被赋值给`globalPerson`。`localPerson` 在`createPerson()`执行完成超出上下文后会自动被解除引用，不需要显式处理。但 `globalPerson` 是一个全局变量，应该在不再需要时手动解除其引用，最后一行就是这么做的。



不过要注意，解除对一个值的引用并不会自动导致相关内存被回收。解除引用的关键在于确保相关的值已经不在上下文里了，因此它在下次垃圾回收时会被回收。 



1. 通过const 和let 声明提升性能

   `ES6`增加这两个关键字不仅有助于改善代码风格，而且同样有助于改进垃圾回收的过程。因为`const`和`let` 都以块（而非函数）为作用域，所以相比于使用`var`，使用这两个新关键字可能会更早地让垃圾回收程序介入，尽早回收应该回收的内存。在块作用域比函数作用域更早终止的情况下，这就有可能发生。

2. 隐藏类和删除操作

   根据`JavaScript` 所在的运行环境，有时候需要根据浏览器使用的JavaScript 引擎来采取不同的性能优化策略。截至2017 年，`Chrome` 是最流行的浏览器，使用`V8 JavaScript` 引擎。`V8`在将解释后的`JavaScript`代码编译为实际的机器码时会利用“隐藏类”。如果你的代码非常注重性能，那么这一点可能对你很重要。

   根据`JavaScript` 所在的运行环境，有时候需要根据浏览器使用的`JavaScript` 引擎来采取不同的性能优化策略。截至2017 年，Chrome 是最流行的浏览器，使用`V8 JavaScript` 引擎。`V8` 在将解释后的`JavaScript`代码编译为实际的机器码时会利用“隐藏类”。如果你的代码非常注重性能，那么这一点可能对你很重要。

   运行期间，`V8` 会将创建的对象与隐藏类关联起来，以跟踪它们的属性特征。能够共享相同隐藏类的对象性能会更好，`V8` 会针对这种情况进行优化，但不一定总能够做到。比如下面的代码： 

   ```js
   function Article() {  
     this.title = 'Inauguration Ceremony Features Kazoo Band'; 
   } 
    
   let a1 = new Article(); 
   let a2 = new Article();
   ```

   V8 会在后台配置，让这两个类实例共享相同的隐藏类，因为这两个实例共享同一个构造函数和原型。假设之后又添加了下面这行代码：

   ```js
   a2.author = 'Jake'; 
   ```

   此时两个 `Article` 实例就会对应两个不同的隐藏类。根据这种操作的频率和隐藏类的大小，这有可能对性能产生明显影响。 

   当然，解决方案就是避免`JavaScript` 的“先创建再补充”（`ready-fire-aim`）式的动态属性赋值，并在构造函数中一次性声明所有属性，如下所示：

   ```js
   function Article(opt_author) {  
     this.title = 'Inauguration Ceremony Features Kazoo Band'; 
     this.author = opt_author; 
   } 
   
    
   let a1 = new Article(); 
   let a2 = new Article('Jake');
   ```

   这样，两个实例基本上就一样了（不考虑`hasOwnProperty` 的返回值），因此可以共享一个隐藏类，从而带来潜在的性能提升。不过要记住，使用`delete` 关键字会导致生成相同的隐藏类片段。看一下这个例子：

   ```js
   function Article() {  
     this.title = 'Inauguration Ceremony Features Kazoo Band'; 
     this.author = 'Jake'; 
   } 
    
   let a1 = new Article(); 
   let a2 = new Article(); 
   
   delete a1.author;
   ```

   在代码结束后，即使两个实例使用了同一个构造函数，它们也不再共享一个隐藏类。动态删除属性与动态添加属性导致的后果一样。最佳实践是把不想要的属性设置为 `null`。这样可以保持隐藏类不变和继续共享，同时也能达到删除引用值供垃圾回收程序回收的效果。比如：

   ```js
   function Article() {  
     this.title = 'Inauguration Ceremony Features Kazoo Band'; 
   
     this.author = 'Jake'; 
   } 
    
   let a1 = new Article(); 
   let a2 = new Article(); 
   
   a1.author = null;
   ```

3. 内存泄漏

   写得不好的 `JavaScript` 可能出现难以察觉且有害的内存泄漏问题。在内存有限的设备上，或者在函数会被调用很多次的情况下，内存泄漏可能是个大问题。`JavaScript` 中的内存泄漏大部分是由不合理的引用导致的。

   意外声明全局变量是最常见但也最容易修复的内存泄漏问题。下面的代码没有使用任何关键字声明变量：

   ```js
   function setName() { 
     name = 'Jake';  
   }
   ```

   此时，解释器会把变量 `name` 当作 `window` 的属性来创建（相当于` window.name = 'Jake'`）。可想而知，在 `window` 对象上创建的属性，只要 `window` 本身不被清理就不会消失。这个问题很容易解决，只要在变量声明前头加上`var`、`let` 或`const` 关键字即可，这样变量就会在函数执行完毕后离开作用域。

   定时器也可能会悄悄地导致内存泄漏。下面的代码中，定时器的回调通过闭包引用了外部变量：

   ```js
   let name = 'Jake'; 
   setInterval(() => { 
     console.log(name);  
   
   }, 100);
   ```

   只要定时器一直运行，回调函数中引用的`name` 就会一直占用内存。垃圾回收程序当然知道这一点，因而就不会清理外部变量。

   

   使用`JavaScript` 闭包很容易在不知不觉间造成内存泄漏。请看下面的例子：

   ```js
   let outer = function() { 
     let name = 'Jake';  
     return function() { 
       return name; 
     }; 
   
   };
   ```

   调用`outer()`会导致分配给name 的内存被泄漏。以上代码执行后创建了一个内部闭包，只要返回的函数存在就不能清理 `name`，因为闭包一直在引用着它。假如 `name` 的内容很大（不止是一个小字串），那可能就是个大问题了。

4. 静态分配与对象池

   为了提升 `JavaScript` 性能，最后要考虑的一点往往就是压榨浏览器了。此时，一个关键问题就是如何减少浏览器执行垃圾回收的次数。开发者无法直接控制什么时候开始收集垃圾，但可以间接控制触发垃圾回收的条件。理论上，如果能够合理使用分配的内存，同时避免多余的垃圾回收，那就可以保住因释放内存而损失的性能。

   

   浏览器决定何时运行垃圾回收程序的一个标准就是对象更替的速度。如果有很多对象被初始化，然后一下子又都超出了作用域，那么浏览器就会采用更激进的方式调度垃圾回收程序运行，这样当然会影响性能。看一看下面的例子，这是一个计算二维矢量加法的函数：

   ```js
   function addVector(a, b) { 
     let resultant = new Vector(); 
     resultant.x = a.x + b.x; 
     resultant.y = a.y + b.y; 
   
     return resultant;  
   
   } 
   ```

   调用这个函数时，会在堆上创建一个新对象，然后修改它，最后再把它返回给调用者。如果这个矢量对象的生命周期很短，那么它会很快失去所有对它的引用，成为可以被回收的值。假如这个矢量加法函数频繁被调用，那么垃圾回收调度程序会发现这里对象更替的速度很快，从而会更频繁地安排垃圾回收。

   

   该问题的解决方案是不要动态创建矢量对象，比如可以修改上面的函数，让它使用一个已有的矢量对象：

   ```js
   function addVector(a, b, resultant) { 
     resultant.x = a.x + b.x;  
     resultant.y = a.y + b.y; 
     return resultant; 
   }
   ```

   当然，这需要在其他地方实例化矢量参数 resultant，但这个函数的行为没有变。那么在哪里创建矢量可以不让垃圾回收调度程序盯上呢？

   

   一个策略是使用对象池。在初始化的某一时刻，可以创建一个对象池，用来管理一组可回收的对象。应用程序可以向这个对象池请求一个对象、设置其属性、使用它，然后在操作完成后再把它还给对象池。**由于没发生对象初始化，垃圾回收探测就不会发现有对象更替，因此垃圾回收程序就不会那么频繁地运行**。下面是一个对象池的伪实现：

   ```js
   // vectorPool 是已有的对象池  
   let v1 = vectorPool.allocate(); 
   let v2 = vectorPool.allocate(); 
   let v3 = vectorPool.allocate(); 
   
    
   v1.x = 10; 
   v1.y = 5; 
   
   v2.x = -3; 
   v2.y = -6; 
    
   
   addVector(v1, v2, v3); 
    
   console.log([v3.x, v3.y]); // [7, -1] 
   
    
   vectorPool.free(v1); 
   vectorPool.free(v2); 
   
   vectorPool.free(v3); 
    
   // 如果对象有属性引用了其他对象 
   
   // 则这里也需要把这些属性设置为null 
   v1 = null; 
   
   v2 = null; 
   v3 = null; 
   ```

   

   如果对象池只按需分配矢量（在对象不存在时创建新的，在对象存在时则复用存在的），那么这个实现本质上是一种贪婪算法，有单调增长但为静态的内存。这个对象池必须使用某种结构维护所有对象，数组是比较好的选择。不过，使用数组来实现，必须留意不要招致额外的垃圾回收。比如下面这个例子：

   ```js
   let vectorList = new Array(100); 
   let vector = new Vector(); 
   
   vectorList.push(vector);
   ```

   由于 JavaScript 数组的大小是动态可变的，引擎会删除大小为 100 的数组，再创建一个新的大小为200 的数组。垃圾回收程序会看到这个删除操作，说不定因此很快就会跑来收一次垃圾。要避免这种动态分配操作，可以在初始化时就创建一个大小够用的数组，从而避免上述先删除再创建的操作。不过，必须事先想好这个数组有多大。

   

   静态分配是优化的一种极端形式。如果应用程序被垃圾回收严重地拖了后腿，可以利用它提升性能。但这种情况并不多见。**大多数情况下，这都属于过早优化，因此不用考虑**。



