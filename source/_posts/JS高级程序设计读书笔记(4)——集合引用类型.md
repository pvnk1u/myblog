---
title: "JS高级程序设计读书笔记(4)——集合引用类型" 
date: 2022-09-07T12:32:39+08:00
draft: false
tags: ["web","javascript"]
categories:
  - "web"
  - "javascript"
toc: true
---



# Object

到目前为止，大多数引用值的示例使用的是 `Object` 类型。`Object` 是 `ECMAScript` 中最常用的类型之一。虽然 `Object` 的实例没有多少功能，但很适合存储和在应用程序间交换数据。



显式地创建 `Object` 的实例有两种方式。第一种是使用 `new` 操作符和 `Object` 构造函数，如下所示：

```js
let person = new Object(); 
person.name = "Nicholas"; 
person.age = 29; 
```

另一种方式是使用对象字面量（`object literal`）表示法。对象字面量是对象定义的简写形式，目的是 为了简化包含大量属性的对象的创建。比如，下面的代码定义了与前面示例相同的 `person` 对象，但使 用的是对象字面量表示法:

```js
let person = { 
 name: "Nicholas", 
 age: 29 
};
```

当然也可以用对象字面量表示法来定义一个只有默认属性和方法的对象，只要使用一对大括号，中间留空就行了：

```js
let person = {}; // 与 new Object()相同
person.name = "Nicholas"; 
person.age = 29; 
```

虽然使用哪种方式创建 `Object` 实例都可以，但实际上开发者更倾向于使用对象字面量表示法。这 是因为对象字面量代码更少，看起来也更有封装所有相关数据的感觉。事实上，对象字面量已经成为给 函数传递大量可选参数的主要方式，比如：

```js
function displayInfo(args) { 
 let output = ""; 
 if (typeof args.name == "string"){ 
 output += "Name: " + args.name + "\n"; 
 } 
 if (typeof args.age == "number") { 
 output += "Age: " + args.age + "\n"; 
 } 
 alert(output); 
} 
displayInfo({ 
 name: "Nicholas", 
 age: 29 
}); 
displayInfo({ 
 name: "Greg" 
}); 
```

虽然属性一般是通过点语法来存取的，这也是面向对象语言的惯例，但也可以使用中括号来存取属性。在使用中括号时，要在括号内使用属性名的字符串形式，比如：

```js
console.log(person["name"]); // "Nicholas" 
console.log(person.name); // "Nicholas" 
```

从功能上讲，这两种存取属性的方式没有区别。使用中括号的主要优势就是可以通过变量访问属性， 就像下面这个例子中一样：

```js
let propertyName = "name"; 
console.log(person[propertyName]); // "Nicholas"
```

另外，如果属性名中包含可能会导致语法错误的字符，或者包含关键字/保留字时，也可以使用中 括号语法。比如：

```js
person["first name"] = "Nicholas";
```

因为"first name"中包含一个空格，所以不能使用点语法来访问。不过，属性名中是可以包含非 字母数字字符的，这时候只要用中括号语法存取它们就行了。





# Array

除了 `Object`，`Array` 应该就是 `ECMAScript` 中最常用的类型了。`ECMAScript` 数组跟其他编程语言 的数组有很大区别。跟其他语言中的数组一样，`ECMAScript` 数组也是一组有序的数据，但跟其他语言 不同的是，数组中每个槽位可以存储任意类型的数据。这意味着可以创建一个数组，它的第一个元素 是字符串，第二个元素是数值，第三个是对象。`ECMAScript` 数组也是动态大小的，会随着数据添加而自动增长。



## 创建数组

有几种基本的方式可以创建数组。一种是使用 `Array` 构造函数，比如：

```js
let colors = new Array();
```



如果知道数组中元素的数量，那么可以给构造函数传入一个数值，然后 `length` 属性就会被自动创 建并设置为这个值。比如，下面的代码会创建一个初始 `length `为 20 的数组：

```js
let colors = new Array(20); 
```

也可以给 `Array` 构造函数传入要保存的元素。比如，下面的代码会创建一个包含 3 个字符串值的 数组：

```js
let colors = new Array("red", "blue", "green"); 
```



在使用 Array 构造函数时，也可以省略 new 操作符。结果是一样的。



另一种创建数组的方式是使用数组字面量（array literal）表示法。数组字面量是在中括号中包含以 逗号分隔的元素列表，如下面的例子所示：

```js
let colors = ["red", "blue", "green"]; // 创建一个包含 3 个元素的数组
let names = []; // 创建一个空数组
let values = [1,2,]; // 创建一个包含 2 个元素的数组
```



`Array` 构造函数还有两个 `ES6` 新增的用于创建数组的静态方法：`from()`和 `of()`。`from()`用于将 类数组结构转换为数组实例，而 `of()`用于将一组参数转换为数组实例。

`Array.from()`的第一个参数是一个类数组对象，即任何可迭代的结构，或者有一个 `length` 属性 和可索引元素的结构。这种方式可用于很多场合：

```js
// 字符串会被拆分为单字符数组
console.log(Array.from("Matt")); // ["M", "a", "t", "t"] 
// 可以使用 from()将集合和映射转换为一个新数组
const m = new Map().set(1, 2) 
 .set(3, 4); 
const s = new Set().add(1) 
 .add(2) 
 .add(3) 
 .add(4); 
console.log(Array.from(m)); // [[1, 2], [3, 4]] 
console.log(Array.from(s)); // [1, 2, 3, 4] 
// Array.from()对现有数组执行浅复制
const a1 = [1, 2, 3, 4]; 
const a2 = Array.from(a1); 
console.log(a1); // [1, 2, 3, 4] 
alert(a1 === a2); // false
// 可以使用任何可迭代对象
const iter = { 
 *[Symbol.iterator]() { 
 yield 1; 
 yield 2; 
 yield 3; 
 yield 4; 
 } 
}; 
console.log(Array.from(iter)); // [1, 2, 3, 4]
// arguments 对象可以被轻松地转换为数组
function getArgsArray() { 
 return Array.from(arguments); 
} 
console.log(getArgsArray(1, 2, 3, 4)); // [1, 2, 3, 4] 
// from()也能转换带有必要属性的自定义对象
const arrayLikeObject = { 
 0: 1, 
 1: 2, 
 2: 3, 
 3: 4, 
 length: 4 
}; 
console.log(Array.from(arrayLikeObject)); // [1, 2, 3, 4] 
```



`Array.from()`还接收第二个可选的映射函数参数。这个函数可以直接增强新数组的值，而无须像 调用 `Array.from().map()`那样先创建一个中间数组。还可以接收第三个可选参数，用于指定映射函 数中 `this` 的值。但这个重写的 `this` 值在箭头函数中不适用。

```js
const a1 = [1, 2, 3, 4]; 
const a2 = Array.from(a1, x => x**2); 
const a3 = Array.from(a1, function(x) {return x**this.exponent}, {exponent: 2}); 
console.log(a2); // [1, 4, 9, 16] 
console.log(a3); // [1, 4, 9, 16] 
```

`Array.of()`可以把一组参数转换为数组。这个方法用于替代在 `ES6`之前常用的 `Array.prototype.slice.call(arguments)`，一种异常笨拙的将 `arguments` 对象转换为数组的写法：



```js
console.log(Array.of(1, 2, 3, 4)); // [1, 2, 3, 4] 
console.log(Array.of(undefined)); // [undefined]
```





## 检测数组

一个经典的 `ECMAScript` 问题是判断一个对象是不是数组。在只有一个网页（因而只有一个全局作用域）的情况下，使用 `instanceof` 操作符就足矣：

```js
if (value instanceof Array){ 
 // 操作数组
} 
```

使用 `instanceof` 的问题是假定只有一个全局执行上下文。如果网页里有多个框架，则可能涉及两 个不同的全局执行上下文，因此就会有两个不同版本的 Array 构造函数。如果要把数组从一个框架传给另一个框架，则这个数组的构造函数将有别于在第二个框架内本地创建的数组。



为解决这个问题，`ECMAScript` 提供了 `Array.isArray()`方法。这个方法的目的就是确定一个值是 否为数组，而不用管它是在哪个全局执行上下文中创建的。来看下面的例子：

```js
if (Array.isArray(value)){ 
 // 操作数组
}
```

## 迭代器方法

在 `ES6` 中，`Array` 的原型上暴露了 3 个用于检索数组内容的方法：`keys()`、`values()`和 `entries()`。`keys()`返回数组索引的迭代器，`values()`返回数组元素的迭代器，而 `entries()`返回 索引/值对的迭代器：

```js
const a = ["foo", "bar", "baz", "qux"]; 
// 因为这些方法都返回迭代器，所以可以将它们的内容
// 通过 Array.from()直接转换为数组实例
const aKeys = Array.from(a.keys()); 
const aValues = Array.from(a.values()); 
const aEntries = Array.from(a.entries()); 
console.log(aKeys); // [0, 1, 2, 3] 
console.log(aValues); // ["foo", "bar", "baz", "qux"] 
console.log(aEntries); // [[0, "foo"], [1, "bar"], [2, "baz"], [3, "qux"]] 
```

使用 `ES6` 的解构可以非常容易地在循环中拆分键/值对：

```js
const a = ["foo", "bar", "baz", "qux"]; 
for (const [idx, element] of a.entries()) { 
 alert(idx); 
 alert(element); 
} 
// 0 
// foo 
// 1 
// bar 
// 2 
// baz 
// 3 
// qux
```



## 复制和填充方法

`ES6` 新增了两个方法：批量复制方法 `copyWithin()`，以及填充数组方法 `fill()`。这两个方法的函数签名类似，都需要指定既有数组实例上的一个范围，包含开始索引，不包含结束索引。使用这个方法不会改变数组的大小。



使用 `fill()`方法可以向一个已有的数组中插入全部或部分相同的值。开始索引用于指定开始填充 的位置，它是可选的。如果不提供结束索引，则一直填充到数组末尾。负值索引从数组末尾开始计算。 也可以将负索引想象成数组长度加上它得到的一个正索引：

```js
const zeroes = [0, 0, 0, 0, 0]; 
// 用 5 填充整个数组
zeroes.fill(5); 
console.log(zeroes); // [5, 5, 5, 5, 5] 
zeroes.fill(0); // 重置
// 用 6 填充索引大于等于 3 的元素
zeroes.fill(6, 3); 
console.log(zeroes); // [0, 0, 0, 6, 6] 
zeroes.fill(0); // 重置
// 用 7 填充索引大于等于 1 且小于 3 的元素
zeroes.fill(7, 1, 3); 
console.log(zeroes); // [0, 7, 7, 0, 0]; 
zeroes.fill(0); // 重置
// 用 8 填充索引大于等于 1 且小于 4 的元素
// (-4 + zeroes.length = 1) 
// (-1 + zeroes.length = 4) 
zeroes.fill(8, -4, -1); 
console.log(zeroes); // [0, 8, 8, 8, 0]; 
fill()静默忽略超出数组边界、零长度及方向相反的索引范围：
const zeroes = [0, 0, 0, 0, 0];  
// 索引过低，忽略
zeroes.fill(1, -10, -6); 
console.log(zeroes); // [0, 0, 0, 0, 0] 
// 索引过高，忽略
zeroes.fill(1, 10, 15); 
console.log(zeroes); // [0, 0, 0, 0, 0] 
// 索引反向，忽略
zeroes.fill(2, 4, 2); 
console.log(zeroes); // [0, 0, 0, 0, 0] 
// 索引部分可用，填充可用部分
zeroes.fill(4, 3, 10) 
console.log(zeroes); // [0, 0, 0, 4, 4] 
```

与 `fill()`不同，`copyWithin()`会按照指定范围浅复制数组中的部分内容，然后将它们插入到指 定索引开始的位置。开始索引和结束索引则与 `fill()`使用同样的计算方法：

```js
let ints, 
 reset = () => ints = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]; 
reset(); 
// 从 ints 中复制索引 0 开始的内容，插入到索引 5 开始的位置
// 在源索引或目标索引到达数组边界时停止
ints.copyWithin(5); 
console.log(ints); // [0, 1, 2, 3, 4, 0, 1, 2, 3, 4] 
reset(); 
// 从 ints 中复制索引 5 开始的内容，插入到索引 0 开始的位置
ints.copyWithin(0, 5); 
console.log(ints); // [5, 6, 7, 8, 9, 5, 6, 7, 8, 9] 
reset(); 
// 从 ints 中复制索引 0 开始到索引 3 结束的内容
// 插入到索引 4 开始的位置
ints.copyWithin(4, 0, 3); 
alert(ints); // [0, 1, 2, 3, 0, 1, 2, 7, 8, 9] 
reset(); 
// JavaScript 引擎在插值前会完整复制范围内的值
// 因此复制期间不存在重写的风险
ints.copyWithin(2, 0, 6); 
alert(ints); // [0, 1, 0, 1, 2, 3, 4, 5, 8, 9] 
reset(); 
// 支持负索引值，与 fill()相对于数组末尾计算正向索引的过程是一样的
ints.copyWithin(-4, -7, -3); 
alert(ints); // [0, 1, 2, 3, 4, 5, 3, 4, 5, 6] 
copyWithin()静默忽略超出数组边界、零长度及方向相反的索引范围：
let ints, 
 reset = () => ints = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]; 
reset(); 
// 索引过低，忽略
ints.copyWithin(1, -15, -12); 
alert(ints); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]; 
reset() 
// 索引过高，忽略
ints.copyWithin(1, 12, 15); 
alert(ints); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]; 
reset(); 
// 索引反向，忽略
ints.copyWithin(2, 4, 2); 
alert(ints); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]; 
reset(); 
// 索引部分可用，复制、填充可用部分
ints.copyWithin(4, 7, 10) 
alert(ints); // [0, 1, 2, 3, 7, 8, 9, 7, 8, 9];
```

## 转换方法

前面提到过，所有对象都有 `toLocaleString()`、`toString()`和 `valueOf()`方法。其中，`valueOf()` 返回的还是数组本身。而 `toString()`返回由数组中每个值的等效字符串拼接而成的一个逗号分隔的 字符串。**也就是说，对数组的每个值都会调用其 `toString()`方法，以得到最终的字符串**。来看下面的例子：

```js
let colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
alert(colors.toString()); // red,blue,green 
alert(colors.valueOf()); // red,blue,green 
alert(colors); // red,blue,green 
```

首先是被显式调用的 `toString()`和 `valueOf()`方法，它们分别返回了数组的字符串表示，即将所有字符串组合起来，以逗号分隔。最后一行代码直接用 `alert()`显示数组，因为 `alert()`期待字符串，所以会在后台调用数组的 `toString()`方法，从而得到跟前面一样的结果。

`toLocaleString()`方法也可能返回跟`toString()`和 `valueOf()`相同的结果，但也不一定。在 调用数组的 `toLocaleString()`方法时，会得到一个逗号分隔的数组值的字符串。它与另外两个方法 唯一的区别是，为了得到最终的字符串，会调用数组每个值的 `toLocaleString()`方法，而不是 `toString()`方法。看下面的例子：

```js
let person1 = { 
 toLocaleString() { 
 	return "Nikolaos"; 
 }, 
 toString() { 
 	return "Nicholas"; 
 } 
}; 
let person2 = { 
 toLocaleString() { 
 	return "Grigorios"; 
 }, 
 toString() { 
 	return "Greg"; 
 } 
}; 
let people = [person1, person2]; 
alert(people); // Nicholas,Greg 
alert(people.toString()); // Nicholas,Greg 
alert(people.toLocaleString()); // Nikolaos,Grigorios
```

这里定义了两个对象 `person1` 和 `person2`，它们都定义了 `toString()`和 `toLocaleString()`方 法，而且返回不同的值。然后又创建了一个包含这两个对象的数组 `people`。在将数组传给 `alert()`时， 输出的是"Nicholas,Greg"，这是因为会在数组每一项上调用 `toString()`方法（与下一行显式调用 `toString()`方法结果一样）。而在调用数组的 `toLocaleString()`方法时，结果变成了"Nikolaos,  Grigorios"，这是因为调用了数组每一项的 `toLocaleString()`方法。

继承的方法 `toLocaleString()`以及 `toString()`都返回数组值的逗号分隔的字符串。如果想使 用不同的分隔符，则可以使用 `join()`方法。`join()`方法接收一个参数，即字符串分隔符，返回包含所 有项的字符串。来看下面的例子：

```js
let colors = ["red", "green", "blue"]; 
alert(colors.join(",")); // red,green,blue 
alert(colors.join("||")); // red||green||blue
```

这里在 colors 数组上调用了 join()方法，得到了与调用 `toString()`方法相同的结果。传入逗 号，结果就是逗号分隔的字符串。最后一行给 `join()` 传入了双竖线，得到了字符串 "red||green||blue"。如果不给 `join()`传入任何参数，或者传入 `undefined`，则仍然使用逗号作为分隔符。



如果数组中某一项是 `null` 或 `undefined`，则在 `join()`、`toLocaleString()`、 `toString()`和 `valueOf()`返回的结果中会以空字符串表示。



## 栈方法

`ECMAScript` 给数组提供几个方法，让它看起来像是另外一种数据结构。数组对象可以像栈一样， 也就是一种限制插入和删除项的数据结构。栈是一种后进先出（LIFO，Last-In-First-Out）的结构，也就 是最近添加的项先被删除。数据项的插入（称为推入，push）和删除（称为弹出，pop）只在栈的一个 地方发生，即栈顶。`ECMAScript` 数组提供了 `push()`和 `pop()`方法，以实现类似栈的行为。



`push()`方法接收任意数量的参数，并将它们添加到数组末尾，返回数组的最新长度。`pop()`方法则 用于删除数组的最后一项，同时减少数组的 `length` 值，返回被删除的项。来看下面的例子：

```js
let colors = new Array(); // 创建一个数组
let count = colors.push("red", "green"); // 推入两项
alert(count); // 2 
count = colors.push("black"); // 再推入一项
alert(count); // 3 
let item = colors.pop(); // 取得最后一项
alert(item); // black 
alert(colors.length); // 2 
```



## 队列方法

就像栈是以 LIFO 形式限制访问的数据结构一样，队列以先进先出（FIFO，First-In-First-Out）形式 限制访问。队列在列表末尾添加数据，但从列表开头获取数据。因为有了在数据末尾添加数据的 `push() `方法，所以要模拟队列就差一个从数组开头取得数据的方法了。这个数组方法叫 `shift()`，它会删除数 组的第一项并返回它，然后数组长度减 1。使用 `shift()`和 `push()`，可以把数组当成队列来使用：

```js
let colors = new Array(); // 创建一个数组
let count = colors.push("red", "green"); // 推入两项
alert(count); // 2 
count = colors.push("black"); // 再推入一项
alert(count); // 3 
let item = colors.shift(); // 取得第一项
alert(item); // red 
alert(colors.length); // 2 
```

`ECMAScript` 也为数组提供了 `unshift()`方法。顾名思义，`unshift()`就是执行跟 `shift()`相反的 操作：在数组开头添加任意多个值，然后返回新的数组长度。通过使用 `unshift()`和 `pop()`，可以在 相反方向上模拟队列，即在数组开头添加新数据，在数组末尾取得数据，如下例所示：

```js
let colors = new Array(); // 创建一个数组
let count = colors.unshift("red", "green"); // 从数组开头推入两项
alert(count); // 2 
count = colors.unshift("black"); // 再推入一项
alert(count); // 3 
let item = colors.pop(); // 取得最后一项
alert(item); // green 
alert(colors.length); // 2 
```



## 排序方法

数组有两个方法可以用来对元素重新排序：`reverse()`和 `sort()`。顾名思义，`reverse()`方法就 是将数组元素反向排列。比如：

```js
let values = [1, 2, 3, 4, 5]; 
values.reverse(); 
alert(values); // 5,4,3,2,1 
```

这里，数组 values 的初始状态为[1,2,3,4,5]。通过调用 reverse()反向排序，得到了 [5,4,3,2,1]。这个方法很直观，但不够灵活，所以才有了 `sort()`方法。



默认情况下，`sort()`会按照升序重新排列数组元素，即最小的值在前面，最大的值在后面。为此， `sort()`会在每一项上调用 `String()`转型函数，然后比较字符串来决定顺序。即使数组的元素都是数值， 也会先把数组转换为字符串再比较、排序。比如：

```js
let values = [0, 1, 5, 10, 15]; 
values.sort(); 
alert(values); // 0,1,10,15,5
```



一开始数组中数值的顺序是正确的，但调用 `sort()`会按照这些数值的字符串形式重新排序。因此， 即使 5 小于 10，但字符串"10"在字符串"5"的前头，所以 10 还是会排到 5 前面。很明显，这在多数情 况下都不是最合适的。为此，`sort()`方法可以接收一个比较函数，用于判断哪个值应该排在前面。

比较函数接收两个参数，如果第一个参数应该排在第二个参数前面，就返回负值；如果两个参数相 等，就返回 0；如果第一个参数应该排在第二个参数后面，就返回正值。下面是使用简单比较函数的一 个例子：

```js
function compare(value1, value2) { 
 if (value1 < value2) { 
 	return -1; 
 } else if (value1 > value2) { 
 	return 1; 
 } else { 
 	return 0; 
 } 
}
```

这个比较函数可以适用于大多数数据类型，可以把它当作参数传给 `sort()`方法，如下所示:

```js
let values = [0, 1, 5, 10, 15]; 
values.sort(compare); 
alert(values); // 0,1,5,10,15 
```

在给 `sort()`方法传入比较函数后，数组中的数值在排序后保持了正确的顺序。当然，比较函数也可以产生降序效果，只要把返回值交换一下即可：

```js
function compare(value1, value2) { 
 if (value1 < value2) { 
 	return 1; 
 } else if (value1 > value2) { 
 	return -1; 
 } else { 
 	return 0; 
 } 
} 
let values = [0, 1, 5, 10, 15]; 
values.sort(compare); 
alert(values); // 15,10,5,1,0 
```

此外，这个比较函数还可简写为一个箭头函数：

```js
let values = [0, 1, 5, 10, 15]; 
values.sort((a, b) => a < b ? 1 : a > b ? -1 : 0); 
alert(values); // 15,10,5,1,0
```

`reverse()`和 `sort()`都返回调用它们的数组的引用。



如果数组的元素是数值，或者是其 `valueOf()`方法返回数值的对象（如 `Date` 对象），这个比较函 数还可以写得更简单，因为这时可以直接用第二个值减去第一个值：

```js
function compare(value1, value2){ 
 return value2 - value1; 
} 
```

比较函数就是要返回小于 0、0 和大于 0 的数值，因此减法操作完全可以满足要求。



## 操作方法

对于数组中的元素，有很多操作方法。比如，`concat()`方法可以在现有数组全部元素基础上 创建一个新数组。它首先会创建一个当前数组的副本，然后再把它的参数添加到副本末尾，最后返回这 个新构建的数组。如果传入一个或多个数组，则 `concat()`会把这些数组的每一项都添加到结果数组。 如果参数不是数组，则直接把它们添加到结果数组末尾。来看下面的例子：

```js
let colors = ["red", "green", "blue"]; 
let colors2 = colors.concat("yellow", ["black", "brown"]); 
console.log(colors); // ["red", "green","blue"] 
console.log(colors2); // ["red", "green", "blue", "yellow", "black", "brown"]
```

这里先创建一个包含 3个值的数组 `colors`。然后 `colors` 调用 `concat()`方法，传入字符串"yellow" 和一个包含"black"和"brown"的数组。保存在 `colors2` 中的结果就是["red", "green", "blue",  "yellow", "black", "brown"]。原始数组 `colors` 保持不变。



打平数组参数的行为可以重写，方法是在参数数组上指定一个特殊的符号：`Symbol.isConcatSpreadable`。这个符号能够阻止 `concat()`打平参数数组。相反，把这个值设置为 true 可以强制打平 类数组对象：

```js
let colors = ["red", "green", "blue"]; 
let newColors = ["black", "brown"]; 
let moreNewColors = { 
 [Symbol.isConcatSpreadable]: true, 
 length: 2, 
 0: "pink", 
 1: "cyan" 
}; 
newColors[Symbol.isConcatSpreadable] = false; 
// 强制不打平数组
let colors2 = colors.concat("yellow", newColors); 
// 强制打平类数组对象
let colors3 = colors.concat(moreNewColors); 
console.log(colors); // ["red", "green", "blue"] 
console.log(colors2); // ["red", "green", "blue", "yellow", ["black", "brown"]] 
console.log(colors3); // ["red", "green", "blue", "pink", "cyan"] 
```

接下来，方法 `slice()`用于创建一个包含原有数组中一个或多个元素的新数组。`slice()`方法可以 接收一个或两个参数：返回元素的开始索引和结束索引。如果只有一个参数，则 `slice()`会返回该索引 到数组末尾的所有元素。如果有两个参数，则 `slice()`返回从开始索引到结束索引对应的所有元素，**其中不包含结束索引对应的元素。记住，这个操作不影响原始数组**。来看下面的例子：

```js
let colors = ["red", "green", "blue", "yellow", "purple"]; 
let colors2 = colors.slice(1); 
let colors3 = colors.slice(1, 4); 
alert(colors2); // green,blue,yellow,purple 
alert(colors3); // green,blue,yellow 
```



这里，`colors` 数组一开始有 5 个元素。调用 `slice()`传入 1 会得到包含 4 个元素的新数组。其中不包括"red"，这是因为拆分操作要从位置 1 开始，即从"green"开始。得到的 `colors2` 数组包含 "green"、"blue"、"yellow"和"purple"。`colors3` 数组是通过调用 `slice()`并传入 1 和 4 得到的， 即从位置 1 开始复制到位置 3。因此 `colors3` 包含"green"、"blue"和"yellow"。

如果 `slice()`的参数有负值，那么就以数值长度加上这个负值的结果确定位置。比 如，在包含 5 个元素的数组上调用 slice(-2,-1)，就相当于调用 slice(3,4)。如果结 束位置小于开始位置，则返回空数组。



或许最强大的数组方法就属 `splice()`了，使用它的方式可以有很多种。`splice()`的主要目的是 在数组中间插入元素，但有 3 种不同的方式使用这个方法。

1. 删除

   需要给 `splice()`传 2 个参数：要删除的第一个元素的位置和要删除的元素数量。可以从 数组中删除任意多个元素，比如 splice(0, 2)会删除前两个元素。

2. 插入

   需要给 `splice()`传 3 个参数：开始位置、0（要删除的元素数量）和要插入的元素，可以在数组中指定的位置插入元素。第三个参数之后还可以传第四个、第五个参数，乃至任意多 个要插入的元素。比如，splice(2, 0, "red", "green")会从数组位置 2 开始插入字符串 "red"和"green"。

3. 替换

   `splice()`在删除元素的同时可以在指定位置插入新元素，同样要传入 3 个参数：开始位 置、要删除元素的数量和要插入的任意多个元素。要插入的元素数量不一定跟删除的元素数量 一致。比如，splice(2, 1, "red", "green")会在位置 2 删除一个元素，然后从该位置开始 向数组中插入"red"和"green"。



`splice()`方法始终返回这样一个数组，它包含从数组中被删除的元素（如果没有删除元素，则返 回空数组）。以下示例展示了上述 3 种使用方式。

```js
let colors = ["red", "green", "blue"]; 
let removed = colors.splice(0,1); // 删除第一项
alert(colors); // green,blue 
alert(removed); // red，只有一个元素的数组
removed = colors.splice(1, 0, "yellow", "orange"); // 在位置 1 插入两个元素
alert(colors); // green,yellow,orange,blue 
alert(removed); // 空数组
removed = colors.splice(1, 1, "red", "purple"); // 插入两个值，删除一个元素
alert(colors); // green,red,purple,orange,blue 
alert(removed); // yellow，只有一个元素的数组
```



## 搜索和位置方法

`ECMAScript` 提供两类搜索数组的方法：按**严格相等**搜索和按**断言函数**搜索。

1. 严格相等

   `ECMAScript` 提供了 3 个严格相等的搜索方法：`indexOf()`、`lastIndexOf()`和 `includes()`。其中，前两个方法在所有版本中都可用，而第三个方法是 `ECMAScript` 7 新增的。这些方法都接收两个参 数：要查找的元素和一个可选的起始搜索位置。`indexOf()`和 `includes()`方法从数组前头（第一项） 开始向后搜索，而 `lastIndexOf()`从数组末尾（最后一项）开始向前搜索。

   `indexOf()`和 `lastIndexOf()`都返回要查找的元素在数组中的位置，如果没找到则返回-1。 `includes()`返回布尔值，表示是否至少找到一个与指定元素匹配的项。**在比较第一个参数跟数组每一 项时，会使用全等（===）比较，也就是说两项必须严格相等。**下面来看一些例子：

   ```js
   let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1]; 
   alert(numbers.indexOf(4)); // 3 
   alert(numbers.lastIndexOf(4)); // 5 
   alert(numbers.includes(4)); // true 
   alert(numbers.indexOf(4, 4)); // 5 
   alert(numbers.lastIndexOf(4, 4)); // 3 
   alert(numbers.includes(4, 7)); // false 
   let person = { name: "Nicholas" }; 
   let people = [{ name: "Nicholas" }]; 
   let morePeople = [person]; 
   alert(people.indexOf(person)); // -1 
   alert(morePeople.indexOf(person)); // 0 
   alert(people.includes(person)); // false 
   alert(morePeople.includes(person)); // true
   ```

2. 断言函数

   `ECMAScript` 也允许按照定义的断言函数搜索数组，每个索引都会调用这个函数。断言函数的返回 值决定了相应索引的元素是否被认为匹配。

   断言函数接收 3 个参数：元素、索引和数组本身。其中元素是数组中当前搜索的元素，索引是当前 元素的索引，而数组就是正在搜索的数组。断言函数返回真值，表示是否匹配。 `find()`和 `findIndex()`方法使用了断言函数。这两个方法都从数组的最小索引开始。`find()`返回 第一个匹配的元素，`findIndex()`返回第一个匹配元素的索引。这两个方法也都接收第二个可选的参数， 用于指定断言函数内部 `this` 的值。

   ```js
   const people = [ 
    { 
    name: "Matt", 
    age: 27 
    }, 
    { 
    name: "Nicholas", 
    age: 29 
    } 
   ]; 
   alert(people.find((element, index, array) => element.age < 28)); 
   // {name: "Matt", age: 27} 
   alert(people.findIndex((element, index, array) => element.age < 28)); 
   // 0 
   ```

   **找到匹配项后，这两个方法都不再继续搜索。**

   ```js
   const evens = [2, 4, 6]; 
   // 找到匹配后，永远不会检查数组的最后一个元素
   evens.find((element, index, array) => { 
    console.log(element); 
    console.log(index); 
    console.log(array); 
    return element === 4; 
   }); 
   // 2 
   // 0 
   // [2, 4, 6] 
   // 4 
   // 1 
   // [2, 4, 6] 
   ```

   

## 迭代方法

`ECMAScript` 为数组定义了 5 个迭代方法。每个方法接收两个参数：以每一项为参数运行的函数， 以及可选的作为函数运行上下文的作用域对象（影响函数中 this 的值）。传给每个方法的函数接收 3 个参数：数组元素、元素索引和数组本身。因具体方法而异，这个函数的执行结果可能会也可能不会影 响方法的返回值。数组的 5 个迭代方法如下。

1. every()

   对数组每一项都运行传入的函数，如果对每一项函数都返回 true，则这个方法返回 true。

2. filter()

   对数组每一项都运行传入的函数，如果对每一项函数都返回 true，则这个方法返回 true。

3. forEach()

   对数组每一项都运行传入的函数，没有返回值。

4. map()

   对数组每一项都运行传入的函数，返回由每次函数调用的结果构成的数组。

5. some()

   对数组每一项都运行传入的函数，如果有一项函数返回 true，则这个方法返回 true。

**这些方法都不改变调用它们的数组。**



在这些方法中，`every()`和 `some()`是最相似的，都是从数组中搜索符合某个条件的元素。对 `every()` 来说，传入的函数必须对每一项都返回 true，它才会返回 true；否则，它就返回 false。而对 `some()` 来说，只要有一项让传入的函数返回 true，它就会返回 true。下面是一个例子：

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1]; 
let everyResult = numbers.every((item, index, array) => item > 2); 
alert(everyResult); // false 
let someResult = numbers.some((item, index, array) => item > 2); 
alert(someResult); // true
```

下面再看一看 `filter()`方法。这个方法基于给定的函数来决定某一项是否应该包含在它返回的数组中。比如，要返回一个所有数值都大于 2 的数组，可以使用如下代码：

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1]; 
let filterResult = numbers.filter((item, index, array) => item > 2); 
alert(filterResult); // 3,4,5,4,3 
```

这里，调用 filter()返回的数组包含 3、4、5、4、3，因为只有对这些项传入的函数才返回 true。 这个方法非常适合从数组中筛选满足给定条件的元素。



接下来 `map()`方法也会返回一个数组。这个数组的每一项都是对原始数组中同样位置的元素运行传 入函数而返回的结果。例如，可以将一个数组中的每一项都乘以 2，并返回包含所有结果的数组，如下所示：

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1]; 
let mapResult = numbers.map((item, index, array) => item * 2); 
alert(mapResult); // 2,4,6,8,10,8,6,4,2 
```

以上代码返回了一个数组，包含原始数组中每个值乘以 2 的结果。这个方法非常适合创建一个与原 始数组元素一一对应的新数组。

最后，再来看一看 `forEach()`方法。这个方法只会对每一项运行传入的函数，没有返回值。本质上，`forEach()`方法相当于使用 for 循环遍历数组。比如：

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1]; 
numbers.forEach((item, index, array) => { 
 // 执行某些操作 
}); 
```

数组的这些迭代方法通过执行不同操作方便了对数组的处理。



## 归并方法

`ECMAScript` 为数组提供了两个归并方法：`reduce()`和 `reduceRight()`。这两个方法都会迭代数组的所有项，并在此基础上构建一个最终返回值。`reduce()`方法从数组第一项开始遍历到最后一项。 而 `reduceRight()`从最后一项开始遍历至第一项。

这两个方法都接收两个参数：对每一项都会运行的归并函数，以及可选的以之为归并起点的初始值。 传给 `reduce()`和 `reduceRight()`的函数接收 4 个参数：上一个归并值、当前项、当前项的索引和数 组本身。这个函数返回的任何值都会作为下一次调用同一个函数的第一个参数。如果没有给这两个方法 传入可选的第二个参数（作为归并起点值），则第一次迭代将从数组的第二项开始，因此传给归并函数 的第一个参数是数组的第一项，第二个参数是数组的第二项。



可以使用 `reduce()`函数执行累加数组中所有数值的操作，比如：

```js
let values = [1, 2, 3, 4, 5]; 
let sum = values.reduce((prev, cur, index, array) => prev + cur); 
alert(sum); // 15
```

第一次执行归并函数时，`prev` 是 1，`cur` 是 2。第二次执行时，`prev` 是 3（1 + 2），`cur` 是 3（数 组第三项）。如此递进，直到把所有项都遍历一次，最后返回归并结果。

`reduceRight()`方法与之类似，只是方向相反。来看下面的例子：

```js
let values = [1, 2, 3, 4, 5]; 
let sum = values.reduceRight(function(prev, cur, index, array){ 
 return prev + cur; 
}); 
alert(sum); // 15 
```

在这里，第一次调用归并函数时 `prev` 是 5，而 `cur` 是 4。当然，最终结果相同，因为归并操作都是简单的加法。



# 定型数组

定型数组（`typed array`）是 `ECMAScript` 新增的结构，目的是提升向原生库传输数据的效率。实际上，`JavaScript` 并没有“TypedArray”类型，它所指的其实是一种特殊的包含数值类型的数组。为理解如何使 用定型数组，有必要先了解一下它的用途。



## 由来

随着浏览器的流行，不难想象人们会满怀期待地通过它来运行复杂的 3D 应用程序。早在 2006 年， Mozilla、Opera 等浏览器提供商就实验性地在浏览器中增加了用于渲染复杂图形应用程序的编程平台， 无须安装任何插件。其目标是开发一套 `JavaScript API`，从而充分利用 3D 图形 API 和 GPU 加速，以便 在元素上渲染复杂的图形。



1. WebGL

   最后的 JavaScript API 是基于 OpenGL ES（OpenGL for Embedded Systems）2.0 规范的。OpenGL ES 是 OpenGL 专注于 2D 和 3D 计算机图形的子集。这个新 API 被命名为 WebGL（Web Graphics Library）， 于 2011 年发布 1.0 版。有了它，开发者就能够编写涉及复杂图形的应用程序，它会被兼容 WebGL 的浏 览器原生解释执行。

   

   在 WebGL 的早期版本中，因为 JavaScript 数组与原生数组之间不匹配，所以出现了性能问题。图形 驱动程序 API 通常不需要以 JavaScript 默认双精度浮点格式传递给它们的数值，而这恰恰是 JavaScript 数组在内存中的格式。因此，每次 WebGL 与 JavaScript 运行时之间传递数组时，WebGL 绑定都需要在 目标环境分配新数组，以其当前格式迭代数组，然后将数值转型为新数组中的适当格式，而这些要花费 很多时间。

2. 定型数组

   这当然是难以接受的，Mozilla 为解决这个问题而实现了 `CanvasFloatArray`。这是一个提供 JavaScript 接口的、C 语言风格的浮点值数组。`JavaScript` 运行时使用这个类型可以分配、读取和写入数组。 这个数组可以直接传给底层图形驱动程序 API，也可以直接从底层获取到。最终，`CanvasFloatArray` 变成了 `Float32Array`，也就是今天定型数组中可用的第一个“类型”。



## ArrayBuffer

`Float32Array` 实际上是一种“视图”，可以允许 `JavaScript` 运行时访问一块名为 `ArrayBuffer` 的 预分配内存。`ArrayBuffer` 是所有定型数组及视图引用的基本单位。



`ArrayBuffer()`是一个普通的 `JavaScript` 构造函数，可用于在内存中分配特定数量的字节空间。

```js
const buf = new ArrayBuffer(16); // 在内存中分配 16 字节
alert(buf.byteLength); // 16 
```



`ArrayBuffer` 一经创建就不能再调整大小。不过，可以使用 `slice()`复制其全部或部分到一个新实例中：

```js
const buf1 = new ArrayBuffer(16); 
const buf2 = buf1.slice(4, 12); 
alert(buf2.byteLength); // 8
```

`ArrayBuffer` 某种程度上类似于 C++的 `malloc()`，但也有几个明显的区别。

1.  malloc()在分配失败时会返回一个 null 指针。ArrayBuffer 在分配失败时会抛出错误。
2. malloc()可以利用虚拟内存，因此最大可分配尺寸只受可寻址系统内存限制。ArrayBuffer 分配的内存不能超过 Number.MAX_SAFE_INTEGER（253  1）字节。
3. malloc()调用成功不会初始化实际的地址。声明 ArrayBuffer 则会将所有二进制位初始化 为 0
4. 通过 malloc()分配的堆内存除非调用 free()或程序退出，否则系统不能再使用。而通过声明 ArrayBuffer 分配的堆内存可以被当成垃圾回收，不用手动释放。



不能仅通过对 `ArrayBuffer` 的引用就读取或写入其内容。要读取或写入 `ArrayBuffer`，就必须 通过视图。视图有不同的类型，但引用的都是 `ArrayBuffer` 中存储的二进制数据。



## DataView

第一种允许读写 `ArrayBuffer` 的视图是 `DataView`。这个视图专为文件 I/O 和网络 I/O 设计，其 `API` 支持对缓冲数据的高度控制，但相比于其他类型的视图性能也差一些。`DataView` 对缓冲内容没有任何预设，也不能迭代。

必须在对已有的`ArrayBuffer` 读取或写入时才能创建 `DataView` 实例。这个实例可以使用全部或 部分 `ArrayBuffer`，且维护着对该缓冲实例的引用，以及视图在缓冲中开始的位置。

```js
const buf = new ArrayBuffer(16); 
// DataView 默认使用整个 ArrayBuffer 
const fullDataView = new DataView(buf); 
alert(fullDataView.byteOffset); // 0 
alert(fullDataView.byteLength); // 16 
alert(fullDataView.buffer === buf); // true 
// 构造函数接收一个可选的字节偏移量和字节长度
// byteOffset=0 表示视图从缓冲起点开始
// byteLength=8 限制视图为前 8 个字节
const firstHalfDataView = new DataView(buf, 0, 8); 
alert(firstHalfDataView.byteOffset); // 0 
alert(firstHalfDataView.byteLength); // 8 
alert(firstHalfDataView.buffer === buf); // true 
// 如果不指定，则 DataView 会使用剩余的缓冲
// byteOffset=8 表示视图从缓冲的第 9 个字节开始
// byteLength 未指定，默认为剩余缓冲
const secondHalfDataView = new DataView(buf, 8); 
alert(secondHalfDataView.byteOffset); // 8 
alert(secondHalfDataView.byteLength); // 8 
alert(secondHalfDataView.buffer === buf); // true
```



要通过 `DataView` 读取缓冲，还需要几个组件。

- 首先是要读或写的字节偏移量。可以看成 `DataView` 中的某种“地址”。

- `DataView` 应该使用 `ElementType` 来实现 `JavaScript` 的 `Number` 类型到缓冲内二进制格式的转换。
- 最后是内存中值的字节序。默认为大端字节序。



1. ElementType

   `DataView` 对存储在缓冲内的数据类型没有预设。它暴露的 `API` 强制开发者在读、写时指定一个 `ElementType`，然后 `DataView` 就会忠实地为读、写而完成相应的转换。

   `ECMAScript` 6 支持 8 种不同的 `ElementType`（见下表）。

   | ElementType | 字节 | 说明               | 等价的C类型    | 值的范围              |
   | ----------- | ---- | ------------------ | -------------- | --------------------- |
   | Int8        | 1    | 8位有符号整数      | signed char    | -128-127              |
   | Uint8       | 1    | 8位无符号整数      | unsigned char  | 0-255                 |
   | Int16       | 2    | 16位有符号整数     | short          | -32768-32767          |
   | Uint16      | 2    | 16位无符号整数     | unsigned short | 0-65535               |
   | Int32       | 4    | 32位有符号整数     | int            | -2147483648-147483647 |
   | Uint32      | 4    | 32位无符号整数     | unsigned int   | 0-4294967295          |
   | Float32     | 4    | 32位IEEE-754浮点数 | float          | -3.4e+38-+3.4e+38     |
   | Float64     | 8    | 64位IEEE-754浮点数 | double         | -1.7e+308-+1.7e+308   |

   `DataView` 为上表中的每种类型都暴露了 `get` 和 `set` 方法，这些方法使用 `byteOffset`（字节偏移 量）定位要读取或写入值的位置。类型是可以互换使用的，如下例所示：

   ```js
   // 在内存中分配两个字节并声明一个 DataView 
   const buf = new ArrayBuffer(2); 
   const view = new DataView(buf); 
   // 说明整个缓冲确实所有二进制位都是 0 
   // 检查第一个和第二个字符
   alert(view.getInt8(0)); // 0 
   alert(view.getInt8(1)); // 0 
   // 检查整个缓冲
   alert(view.getInt16(0)); // 0 
   // 将整个缓冲都设置为 1 
   // 255 的二进制表示是 11111111（2^8 - 1）
   view.setUint8(0, 255); 
   // DataView 会自动将数据转换为特定的 ElementType 
   // 255 的十六进制表示是 0xFF 
   view.setUint8(1, 0xFF); 
   // 现在，缓冲里都是 1 了
   // 如果把它当成二补数的有符号整数，则应该是-1 
   alert(view.getInt16(0)); // -1
   ```

2. 字节序

   前面例子中的缓冲有意回避了字节序的问题。“字节序”指的是计算系统维护的一种字节顺序的约 定。`DataView` 只支持两种约定：`大端字节序`和`小端字节序`。大端字节序也称为“网络字节序”，意思是最高有效位保存在第一个字节，而最低有效位保存在最后一个字节。小端字节序正好相反，即最低有效位保存在第一个字节，最高有效位保存在最后一个字节。

   

   `JavaScript` 运行时所在系统的原生字节序决定了如何读取或写入字节，但 `DataView` 并不遵守这 个约定。对一段内存而言，`DataView` 是一个中立接口，它会遵循你指定的字节序。`DataView` 的所 有 `API` 方法都以大端字节序作为默认值，但接收一个可选的布尔值参数，设置为 `true` 即可启用小端字节序。

   

   ```js
   // 在内存中分配两个字节并声明一个 DataView 
   const buf = new ArrayBuffer(2); 
   const view = new DataView(buf); 
   // 填充缓冲，让第一位和最后一位都是 1 
   view.setUint8(0, 0x80); // 设置最左边的位等于 1 
   view.setUint8(1, 0x01); // 设置最右边的位等于 1 
   // 缓冲内容（为方便阅读，人为加了空格）
   // 0x8 0x0 0x0 0x1 
   // 1000 0000 0000 0001 
   // 按大端字节序读取 Uint16 
   // 0x80 是高字节，0x01 是低字节
   // 0x8001 = 2^15 + 2^0 = 32768 + 1 = 32769 
   alert(view.getUint16(0)); // 32769 
   // 按小端字节序读取 Uint16 
   // 0x01 是高字节，0x80 是低字节
   // 0x0180 = 2^8 + 2^7 = 256 + 128 = 384 
   alert(view.getUint16(0, true)); // 384 
   // 按大端字节序写入 Uint16 
   view.setUint16(0, 0x0004); 
   // 缓冲内容（为方便阅读，人为加了空格）
   // 0x0 0x0 0x0 0x4 
   // 0000 0000 0000 0100 
   alert(view.getUint8(0)); // 0 
   alert(view.getUint8(1)); // 4 
   // 按小端字节序写入 Uint16 
   view.setUint16(0, 0x0002, true); 
   // 缓冲内容（为方便阅读，人为加了空格）
   // 0x0 0x2 0x0 0x0 
   // 0000 0010 0000 0000 
   alert(view.getUint8(0)); // 2 
   alert(view.getUint8(1)); // 0
   ```

3. 边界情形

   `DataView` 完成读、写操作的前提是必须有充足的缓冲区，否则就会抛出 `RangeError`：

   ```js
   const buf = new ArrayBuffer(6); 
   const view = new DataView(buf); 
   // 尝试读取部分超出缓冲范围的值
   view.getInt32(4); 
   // RangeError 
   // 尝试读取超出缓冲范围的值
   view.getInt32(8); 
   // RangeError 
   // 尝试读取超出缓冲范围的值
   view.getInt32(-1); 
   // RangeError 
   // 尝试写入超出缓冲范围的值
   view.setInt32(4, 123); 
   // RangeError 
   ```

   `DataView` 在写入缓冲里会尽最大努力把一个值转换为适当的类型，后备为 0。如果无法转换，则 抛出错误：

   ```js
   const buf = new ArrayBuffer(1); 
   const view = new DataView(buf); 
   view.setInt8(0, 1.5); 
   alert(view.getInt8(0)); // 1 
   view.setInt8(0, [4]); 
   alert(view.getInt8(0)); // 4 
   view.setInt8(0, 'f'); 
   alert(view.getInt8(0)); // 0 
   view.setInt8(0, Symbol()); 
   // TypeError 
   ```

4. 定型数组

   定型数组是另一种形式的 `ArrayBuffer` 视图。虽然概念上与 `DataView` 接近，但定型数组的区别 在于，它特定于一种 `ElementType` 且遵循系统原生的字节序。相应地，定型数组提供了适用面更广的 `API` 和更高的性能。设计定型数组的目的就是提高与 `WebGL` 等原生库交换二进制数据的效率。由于定 型数组的二进制表示对操作系统而言是一种容易使用的格式，`JavaScript `引擎可以重度优化算术运算、 按位运算和其他对定型数组的常见操作，因此使用它们速度极快。

   创建定型数组的方式包括读取已有的缓冲、使用自有缓冲、填充可迭代结构，以及填充基于任意类 型的定型数组。另外，通过`.from()`和`.of()`也可以创建定型数组：

   ```js
   // 创建一个 12 字节的缓冲
   const buf = new ArrayBuffer(12); 
   // 创建一个引用该缓冲的 Int32Array 
   const ints = new Int32Array(buf); 
   // 这个定型数组知道自己的每个元素需要 4 字节
   // 因此长度为 3 
   alert(ints.length); // 3 
   // 创建一个长度为 6 的 Int32Array 
   const ints2 = new Int32Array(6); 
   // 每个数值使用 4 字节，因此 ArrayBuffer 是 24 字节
   alert(ints2.length); // 6 
   // 类似 DataView，定型数组也有一个指向关联缓冲的引用
   alert(ints2.buffer.byteLength); // 24 
   // 创建一个包含[2, 4, 6, 8]的 Int32Array 
   const ints3 = new Int32Array([2, 4, 6, 8]); 
   alert(ints3.length); // 4 
   alert(ints3.buffer.byteLength); // 16 
   alert(ints3[2]); // 6 
   // 通过复制 ints3 的值创建一个 Int16Array 
   const ints4 = new Int16Array(ints3); 
   // 这个新类型数组会分配自己的缓冲
   // 对应索引的每个值会相应地转换为新格式
   alert(ints4.length); // 4 
   alert(ints4.buffer.byteLength); // 8 
   alert(ints4[2]); // 6 
   // 基于普通数组来创建一个 Int16Array 
   const ints5 = Int16Array.from([3, 5, 7, 9]); 
   alert(ints5.length); // 4 
   alert(ints5.buffer.byteLength); // 8 
   alert(ints5[2]); // 7 
   // 基于传入的参数创建一个 Float32Array 
   const floats = Float32Array.of(3.14, 2.718, 1.618); 
   alert(floats.length); // 3 
   alert(floats.buffer.byteLength); // 12 
   alert(floats[2]); // 1.6180000305175781
   ```

   定型数组的构造函数和实例都有一个 `BYTES_PER_ELEMENT` 属性，返回该类型数组中每个元素的大小：

   ```js
   alert(Int16Array.BYTES_PER_ELEMENT); // 2 
   alert(Int32Array.BYTES_PER_ELEMENT); // 4 
   const ints = new Int32Array(1), 
    	  floats = new Float64Array(1); 
   alert(ints.BYTES_PER_ELEMENT); // 4 
   alert(floats.BYTES_PER_ELEMENT); // 8
   ```

   如果定型数组没有用任何值初始化，则其关联的缓冲会以 0 填充：

   ```js
   const ints = new Int32Array(4); 
   alert(ints[0]); // 0 
   alert(ints[1]); // 0 
   alert(ints[2]); // 0 
   alert(ints[3]); // 0 
   ```

   1. 定型数组行为
   
      从很多方面看，定型数组与普通数组都很相似。定型数组支持如下操作符、方法和属性：
   
      - []
      - copyWithin()
      - entries()
      - every()
      - fill()
      - filter()
      - find()
      - findIndex()
      - forEach()
      - indexOf()
      - join()
      - keys()
      - lastIndexOf()
      - length
      - map()
      - reduce()
      - reduceRight()
      - reverse()
      - slice()
      - some()
      - sort()
      - toLocaleString()
      - toString()
      - values()
   
      其中，返回新数组的方法也会返回包含同样元素类型（element type）的新定型数组：
   
      ```js
      const ints = new Int16Array([1, 2, 3]); 
      const doubleints = ints.map(x => 2*x); 
      alert(doubleints instanceof Int16Array); // true
      ```
   
      定型数组有一个 `Symbol.iterator` 符号属性，因此可以通过 `for..of` 循环和扩展操作符来操作：
   
      ```js
      const ints = new Int16Array([1, 2, 3]); 
      for (const int of ints) { 
       alert(int); 
      } 
      // 1 
      // 2 
      // 3 
      alert(Math.max(...ints)); // 3 
      ```
   
   2. 定型数组同样使用数组缓冲来存储数据，而数组缓冲无法调整大小。因此，下列方法不适用于定型数组：
   
      - concat()
      - pop()
      - push()
      -  shift()
      - splice()
      - unshift()
   
      不过，定型数组也提供了两个新方法，可以快速向外或向内复制数据：`set()`和 `subarray()`。 `set()`从提供的数组或定型数组中把值复制到当前定型数组中指定的索引位置：
   
      ```js
      // 创建长度为 8 的 int16 数组
      const container = new Int16Array(8); 
      // 把定型数组复制为前 4 个值
      // 偏移量默认为索引 0 
      container.set(Int8Array.of(1, 2, 3, 4)); 
      console.log(container); // [1,2,3,4,0,0,0,0] 
      // 把普通数组复制为后 4 个值
      // 偏移量 4 表示从索引 4 开始插入
      container.set([5,6,7,8], 4); 
      console.log(container); // [1,2,3,4,5,6,7,8] 
      // 溢出会抛出错误
      container.set([5,6,7,8], 7); 
      // RangeError 
      ```
   
      `subarray()`执行与 `set()`相反的操作，它会基于从原始定型数组中复制的值返回一个新定型数组。 复制值时的开始索引和结束索引是可选的：
   
      ```js
      const source = Int16Array.of(2, 4, 6, 8); 
      // 把整个数组复制为一个同类型的新数组
      const fullCopy = source.subarray(); 
      console.log(fullCopy); // [2, 4, 6, 8] 
      // 从索引 2 开始复制数组
      const halfCopy = source.subarray(2); 
      console.log(halfCopy); // [6, 8] 
      // 从索引 1 开始复制到索引 3 
      const partialCopy = source.subarray(1, 3); 
      console.log(partialCopy); // [4, 6]
      ```
   
   3. 下溢与上溢
   
      定型数组中值的下溢和上溢不会影响到其他索引，但仍然需要考虑数组的元素应该是什么类型。定型数组对于可以存储的每个索引只接受一个相关位，而不考虑它们对实际数值的影响。以下代码演示了 如何处理下溢和上溢：
   
      ```js
      // 长度为 2 的有符号整数数组
      // 每个索引保存一个二补数形式的有符号整数
      // 范围是-128（-1 * 2^7）~127（2^7 - 1）
      const ints = new Int8Array(2); 
      // 长度为 2 的无符号整数数组
      // 每个索引保存一个无符号整数
      // 范围是 0~255（2^7 - 1）
      const unsignedInts = new Uint8Array(2); 
      // 上溢的位不会影响相邻索引
      // 索引只取最低有效位上的 8 位
      unsignedInts[1] = 256; // 0x100 
      console.log(unsignedInts); // [0, 0] 
      unsignedInts[1] = 511; // 0x1FF 
      console.log(unsignedInts); // [0, 255] 
      // 下溢的位会被转换为其无符号的等价值
      // 0xFF 是以二补数形式表示的-1（截取到 8 位）, 
      // 但 255 是一个无符号整数
      unsignedInts[1] = -1 // 0xFF (truncated to 8 bits) 
      console.log(unsignedInts); // [0, 255] 
      // 上溢自动变成二补数形式
      // 0x80 是无符号整数的 128，是二补数形式的-128 
      ints[1] = 128; // 0x80 
      console.log(ints); // [0, -128] 
      // 下溢自动变成二补数形式
      // 0xFF 是无符号整数的 255，是二补数形式的-1 
      ints[1] = 255; // 0xFF 
      console.log(ints); // [0, -1] 
      ```
   
      除了 8 种元素类型，还有一种“夹板”数组类型：`Uint8ClampedArray`，不允许任何方向溢出。 超出最大值 `255` 的值会被向下舍入为 `255`，而小于最小值 0 的值会被向上舍入为 0。



# Map

`ECMAScript` 6 以前，在 `JavaScript` 中实现“键/值”式存储可以使用 `Object` 来方便高效地完成，也就是使用对象属性作为键，再使用属性来引用值。但这种实现并非没有问题，为此 `TC39` 委员会专门为 “键/值”存储定义了一个规范。

作为 `ECMAScript` 6 的新增特性，`Map` 是一种新的集合类型，为这门语言带来了真正的键/值存储机 制。`Map` 的大多数特性都可以通过 `Object` 类型实现，但二者之间还是存在一些细微的差异。具体实践中使用哪一个，还是值得细细甄别。



## 基本API

使用 `new` 关键字和 `Map` 构造函数可以创建一个空映射：

```js
const m = new Map();
```

如果想在创建的同时初始化实例，可以给 `Map` 构造函数传入一个可迭代对象，需要包含键/值对数 组。可迭代对象中的每个键/值对都会按照迭代顺序插入到新映射实例中：

```js
// 使用嵌套数组初始化映射
const m1 = new Map([ 
 ["key1", "val1"], 
 ["key2", "val2"], 
 ["key3", "val3"] 
]); 
alert(m1.size); // 3 
// 使用自定义迭代器初始化映射
const m2 = new Map({ 
 [Symbol.iterator]: function*() { 
 yield ["key1", "val1"]; 
 yield ["key2", "val2"]; 
 yield ["key3", "val3"]; 
 } 
}); 
alert(m2.size); // 3 
// 映射期待的键/值对，无论是否提供
const m3 = new Map([[]]); 
alert(m3.has(undefined)); // true 
alert(m3.get(undefined)); // undefined
```

初始化之后，可以使用 `set()`方法再添加键/值对。另外，可以使用 `get()`和 `has()`进行查询，可 以通过`size` 属性获取映射中的键/值对的数量，还可以使用 `delete()`和 `clear()`删除值。

```js
const m = new Map(); 
alert(m.has("firstName")); // false 
alert(m.get("firstName")); // undefined 
alert(m.size); // 0 
m.set("firstName", "Matt") 
 .set("lastName", "Frisbie"); 
alert(m.has("firstName")); // true 
alert(m.get("firstName")); // Matt 
alert(m.size); // 2 
m.delete("firstName"); // 只删除这一个键/值对
alert(m.has("firstName")); // false 
alert(m.has("lastName")); // true 
alert(m.size); // 1 
m.clear(); // 清除这个映射实例中的所有键/值对
alert(m.has("firstName")); // false 
alert(m.has("lastName")); // false 
alert(m.size); // 0 
```

`set()`方法返回映射实例，因此可以把多个操作连缀起来，包括初始化声明：

```js
const m = new Map().set("key1", "val1"); 
m.set("key2", "val2") 
 .set("key3", "val3"); 
alert(m.size); // 3
```

与 `Object` 只能使用数值、字符串或符号作为键不同，`Map` 可以使用任何 `JavaScript` 数据类型作为 键。`Map` 内部使用 `SameValueZero` 比较操作（`ECMAScript` 规范内部定义，语言中不能使用），基本上相当于使用严格对象相等的标准来检查键的匹配性。与 `Object` 类似，映射的值是没有限制的。

```js
const m = new Map(); 
const functionKey = function() {}; 
const symbolKey = Symbol(); 
const objectKey = new Object(); 
m.set(functionKey, "functionValue"); 
m.set(symbolKey, "symbolValue"); 
m.set(objectKey, "objectValue"); 
alert(m.get(functionKey)); // functionValue 
alert(m.get(symbolKey)); // symbolValue 
alert(m.get(objectKey)); // objectValue 
// SameValueZero 比较意味着独立实例不冲突
alert(m.get(function() {})); // undefined
```

与严格相等一样，在映射中用作键和值的对象及其他“集合”类型，在自己的内容或属性被修改时仍然保持不变：

```js
const m = new Map(); 
const objKey = {}, 
 objVal = {}, 
 arrKey = [], 
 arrVal = []; 
m.set(objKey, objVal); 
m.set(arrKey, arrVal); 
objKey.foo = "foo"; 
objVal.bar = "bar"; 
arrKey.push("foo"); 
arrVal.push("bar"); 
console.log(m.get(objKey)); // {bar: "bar"} 
console.log(m.get(arrKey)); // ["bar"]
```

`SameValueZero`比较也可能导致意想不到的冲突：

```js
const m = new Map(); 
const a = 0/"", // NaN 
 b = 0/"", // NaN 
 pz = +0, 
 nz = -0; 
alert(a === b); // false 
alert(pz === nz); // true 
m.set(a, "foo"); 
m.set(pz, "bar"); 
alert(m.get(b)); // foo 
alert(m.get(nz)); // bar
```



## 顺序与迭代

与 `Object` 类型的一个主要差异是，`Map` 实例会维护键值对的插入顺序，因此可以根据插入顺序执行迭代操作。

映射实例可以提供一个迭代器（`Iterator`），能以插入顺序生成[key, value]形式的数组。可以 通过 `entries()`方法（或者 `Symbol.iterator` 属性，它引用 `entries()`）取得这个迭代器：

```js
const m = new Map([ 
 ["key1", "val1"], 
 ["key2", "val2"], 
 ["key3", "val3"] 
]); 
alert(m.entries === m[Symbol.iterator]); // true 
for (let pair of m.entries()) { 
 alert(pair); 
} 
// [key1,val1] 
// [key2,val2] 
// [key3,val3] 
for (let pair of m[Symbol.iterator]()) { 
 alert(pair); 
} 
// [key1,val1] 
// [key2,val2] 
// [key3,val3] 
```

因为 `entries()`是默认迭代器，所以可以直接对映射实例使用扩展操作，把映射转换为数组：

```js
const m = new Map([ 
 ["key1", "val1"], 
 ["key2", "val2"], 
 ["key3", "val3"] 
]); 
console.log([...m]); // [[key1,val1],[key2,val2],[key3,val3]] 
```

如果不使用迭代器，而是使用回调方式，则可以调用映射的 `forEach(callback, opt_thisArg)` 方法并传入回调，依次迭代每个键/值对。传入的回调接收可选的第二个参数，这个参数用于重写回调 内部`this` 的值：

```js
const m = new Map([ 
 ["key1", "val1"], 
 ["key2", "val2"], 
 ["key3", "val3"] 
]); 
m.forEach((val, key) => alert(`${key} -> ${val}`)); 
// key1 -> val1 
// key2 -> val2 
// key3 -> val3 
```

`keys()`和 `values()`分别返回以插入顺序生成键和值的迭代器：

```js
const m = new Map([ 
 ["key1", "val1"], 
 ["key2", "val2"], 
 ["key3", "val3"] 
]); 
for (let key of m.keys()) { 
 alert(key); 
} 
// key1 
// key2 
// key3 
for (let key of m.values()) { 
 alert(key); 
} 
// value1 
// value2 
// value3 
```

键和值在迭代器遍历时是可以修改的，但映射内部的引用则无法修改。当然，这并不妨碍修改作为键或值的对象内部的属性，因为这样并不影响它们在映射实例中的身份：

```js
const m1 = new Map([ 
 ["key1", "val1"] 
]); 
// 作为键的字符串原始值是不能修改的
for (let key of m1.keys()) { 
 key = "newKey"; 
 alert(key); // newKey 
 alert(m1.get("key1")); // val1 
} 
const keyObj = {id: 1}; 
const m = new Map([ 
 [keyObj, "val1"] 
]); 
// 修改了作为键的对象的属性，但对象在映射内部仍然引用相同的值
for (let key of m.keys()) { 
 key.id = "newKey"; 
 alert(key); // {id: "newKey"} 
 alert(m.get(keyObj)); // val1 
} 
alert(keyObj); // {id: "newKey"}
```



## 选择 Object 还是 Map

对于多数 Web 开发任务来说，选择 `Object` 还是 `Map` 只是个人偏好问题，影响不大。不过，对于在乎内存和性能的开发者来说，对象和映射之间确实存在显著的差别。

1. 内存占用

   `Object` 和 `Map` 的工程级实现在不同浏览器间存在明显差异，但存储单个键/值对所占用的内存数量 都会随键的数量线性增加。批量添加或删除键/值对则取决于各浏览器对该类型内存分配的工程实现。 不同浏览器的情况不同，但给定固定大小的内存，`Map` 大约可以比 `Object` 多存储 50%的键/值对。

2. 插入性能

   向 `Object` 和 `Map` 中插入新键/值对的消耗大致相当，不过插入 `Map` 在所有浏览器中一般会稍微快 一点儿。对这两个类型来说，插入速度并不会随着键/值对数量而线性增加。如果代码涉及大量插入操 作，那么显然 `Map` 的性能更佳。

3. 查找速度

   与插入不同，从大型 `Object` 和 `Map` 中查找键/值对的性能差异极小，但如果只包含少量键/值对， 则 `Object` 有时候速度更快。在把 `Object` 当成数组使用的情况下（比如使用连续整数作为属性），浏览器引擎可以进行优化，在内存中使用更高效的布局。这对 `Map` 来说是不可能的。对这两个类型而言， 查找速度不会随着键/值对数量增加而线性增加。如果代码涉及大量查找操作，那么某些情况下可能选 择 `Object` 更好一些。

4. 删除性能

   使用 `delete` 删除 `Object` 属性的性能一直以来饱受诟病，目前在很多浏览器中仍然如此。为此， 出现了一些伪删除对象属性的操作，包括把属性值设置为 `undefined` 或 `null`。但很多时候，这都是一 种讨厌的或不适宜的折中。而对大多数浏览器引擎来说，`Map` 的 `delete()`操作都比插入和查找更快。 如果代码涉及大量删除操作，那么毫无疑问应该选择 `Map`。



## WeakMap

`ECMAScript` 6 新增的“弱映射”（`WeakMap`）是一种新的集合类型，为这门语言带来了增强的键/ 值对存储机制。`WeakMap` 是 `Map` 的“兄弟”类型，其 `API` 也是 `Map` 的子集。`WeakMap` 中的“weak”（弱）， 描述的是 `JavaScript` 垃圾回收程序对待“弱映射”中键的方式。



### 基本API

可以使用 new 关键字实例化一个空的 `WeakMap`：

```js
const wm = new WeakMap();
```

弱映射中的键只能是 `Object` 或者继承自 `Object` 的类型，尝试使用非对象设置键会抛出 `TypeError`。值的类型没有限制。



如果想在初始化时填充弱映射，则构造函数可以接收一个可迭代对象，其中需要包含键/值对数组。 可迭代对象中的每个键/值都会按照迭代顺序插入新实例中：

```js
const key1 = {id: 1}, 
 key2 = {id: 2},
  key3 = {id: 3}; 
// 使用嵌套数组初始化弱映射
const wm1 = new WeakMap([ 
 [key1, "val1"], 
 [key2, "val2"], 
 [key3, "val3"] 
]); 
alert(wm1.get(key1)); // val1 
alert(wm1.get(key2)); // val2 
alert(wm1.get(key3)); // val3 
// 初始化是全有或全无的操作
// 只要有一个键无效就会抛出错误，导致整个初始化失败
const wm2 = new WeakMap([ 
 [key1, "val1"], 
 ["BADKEY", "val2"], 
 [key3, "val3"] 
]); 
// TypeError: Invalid value used as WeakMap key 
typeof wm2; 
// ReferenceError: wm2 is not defined 
// 原始值可以先包装成对象再用作键
const stringKey = new String("key1"); 
const wm3 = new WeakMap([ 
 stringKey, "val1" 
]); 
alert(wm3.get(stringKey)); // "val1"
```

初始化之后可以使用 `set()`再添加键/值对，可以使用 `get()`和 `has()`查询，还可以使用 `delete()` 删除：

```js
const wm = new WeakMap(); 
const key1 = {id: 1}, 
 key2 = {id: 2}; 
alert(wm.has(key1)); // false 
alert(wm.get(key1)); // undefined 
wm.set(key1, "Matt") 
 .set(key2, "Frisbie"); 
alert(wm.has(key1)); // true 
alert(wm.get(key1)); // Matt 
wm.delete(key1); // 只删除这一个键/值对
alert(wm.has(key1)); // false 
alert(wm.has(key2)); // true 
```



`set()`方法返回弱映射实例，因此可以把多个操作连缀起来，包括初始化声明：

```js
const key1 = {id: 1}, 
 key2 = {id: 2}, 
 key3 = {id: 3}; 
const wm = new WeakMap().set(key1, "val1"); 
wm.set(key2, "val2") 
 .set(key3, "val3"); 
alert(wm.get(key1)); // val1 
alert(wm.get(key2)); // val2 
alert(wm.get(key3)); // val3 
```



### 弱键

`WeakMap` 中“weak”表示弱映射的键是“弱弱地拿着”的。意思就是，**这些键不属于正式的引用， 不会阻止垃圾回收**。但要注意的是，弱映射中值的引用可不是“弱弱地拿着”的。只要键存在，键/值 对就会存在于映射中，并被当作对值的引用，因此就不会被当作垃圾回收。



来看下面的例子：

```js
const wm = new WeakMap(); 
wm.set({}, "val");
```

`set()`方法初始化了一个新对象并将它用作一个字符串的键。因为没有指向这个对象的其他引用， 所以当这行代码执行完成后，这个对象键就会被当作垃圾回收。然后，这个键/值对就从弱映射中消失 了，使其成为一个空映射。在这个例子中，因为值也没有被引用，所以这对键/值被破坏以后，值本身 也会成为垃圾回收的目标。



再看一个稍微不同的例子：

```js
const wm = new WeakMap(); 
const container = { 
 key: {} 
}; 
wm.set(container.key, "val"); 
function removeReference() { 
 container.key = null; 
} 
```



这一次，`container` 对象维护着一个对弱映射键的引用，因此这个对象键不会成为垃圾回收的目 标。不过，如果调用了 `removeReference()`，就会摧毁键对象的最后一个引用，垃圾回收程序就可以 把这个键/值对清理掉。



### 不可迭代键

因为 `WeakMap` 中的键/值对任何时候都可能被销毁，所以没必要提供迭代其键/值对的能力。当然， 也用不着像 `clear()`这样一次性销毁所有键/值的方法。`WeakMap` 确实没有这个方法。因为不可能迭代， 所以也不可能在不知道对象引用的情况下从弱映射中取得值。即便代码可以访问 `WeakMap` 实例，也没办法看到其中的内容。 

`WeakMap` 实例之所以限制只能用对象作为键，是为了保证只有通过键对象的引用才能取得值。如果允许原始值，那就没办法区分初始化时使用的字符串字面量和初始化之后使用的一个相等的字符串了。



### 使用弱映射

`WeakMap` 实例与现有 `JavaScript` 对象有着很大不同，可能一时不容易说清楚应该怎么使用它。这个问题没有唯一的答案，但已经出现了很多相关策略。

1. 私有变量

   弱映射造就了在JavaScript中实现真正私有变量的一种新方式。前提很明确：私有变量会存储在弱映射中，以对象实例为键，以私有成员的字典为值。

   下面是一个示例实现：

   ```js
   const wm = new WeakMap(); 
   class User { 
    constructor(id) { 
        this.idProperty = Symbol('id'); 
        this.setId(id); 
    } 
    setPrivate(property, value) { 
    	const privateMembers = wm.get(this) || {}; 
    	privateMembers[property] = value; 
    	wm.set(this, privateMembers); 
    } 
    getPrivate(property) { 
    	return wm.get(this)[property]; 
    } 
    setId(id) { 
    	this.setPrivate(this.idProperty, id); 
    } 
    getId() { 
    	return this.getPrivate(this.idProperty); 
    } 
   } 
   const user = new User(123); 
   alert(user.getId()); // 123 
   user.setId(456); 
   alert(user.getId()); // 456
   // 并不是真正私有的
   alert(wm.get(user)[user.idProperty]); // 456 
   ```

   

   慧眼独具的读者会发现，对于上面的实现，外部代码只需要拿到对象实例的引用和弱映射，就可以 取得“私有”变量了。为了避免这种访问，可以用一个闭包把 `WeakMap` 包装起来，这样就可以把弱映射与外界完全隔离开了：

   ```js
   const User = (() => { 
   	const wm = new WeakMap(); 
       class User { 
           constructor(id) { 
               this.idProperty = Symbol('id'); 
               this.setId(id); 
           } 
           setPrivate(property, value) { 
               const privateMembers = wm.get(this) || {}; 
               privateMembers[property] = value; 
               wm.set(this, privateMembers); 
           } 
           getPrivate(property) { 
               return wm.get(this)[property]; 
           } 
           setId(id) { 
               this.setPrivate(this.idProperty, id); 
           } 
           getId(id) { 
               return this.getPrivate(this.idProperty); 
           } 
        } 
    	return User; 
   })(); 
   const user = new User(123); 
   alert(user.getId()); // 123 
   user.setId(456); 
   alert(user.getId()); // 456
   ```

   这样，拿不到弱映射中的键，也就无法取得弱映射中对应的值。虽然这防止了前面提到的访问，但 整个代码也完全陷入了 `ES6` 之前的闭包私有变量模式。

2. 因为 `WeakMap` 实例不会妨碍垃圾回收，所以非常适合保存关联元数据。来看下面这个例子，其中 使用了常规的 `Map`：

   ```js
   const m = new Map(); 
   const loginButton = document.querySelector('#login'); 
   // 给这个节点关联一些元数据
   m.set(loginButton, {disabled: true}); 
   ```

   假设在上面的代码执行后，页面被 `JavaScript` 改变了，原来的登录按钮从 `DOM` 树中被删掉了。但由于映射中还保存着按钮的引用，所以对应的 DOM 节点仍然会逗留在内存中，除非明确将其从映射中 删除或者等到映射本身被销毁。 如果这里使用的是弱映射，如以下代码所示，那么当节点从 DOM 树中被删除后，垃圾回收程序就可以立即释放其内存（假设没有其他地方引用这个对象）：

   ```js
   const wm = new WeakMap(); 
   const loginButton = document.querySelector('#login'); 
   // 给这个节点关联一些元数据
   wm.set(loginButton, {disabled: true});
   ```



# Set

`ECMAScript` 6 新增的 `Set` 是一种新集合类型，为这门语言带来集合数据结构。`Set` 在很多方面都像是加强的 Map，这是因为它们的大多数 `API` 和行为都是共有的。



## 基本API

使用 `new` 关键字和 `Set` 构造函数可以创建一个空集合：

```js
const m = new Set();
```

如果想在创建的同时初始化实例，则可以给 Set 构造函数传入一个可迭代对象，其中需要包含插入到新集合实例中的元素：

```js
// 使用数组初始化集合 
const s1 = new Set(["val1", "val2", "val3"]); 
alert(s1.size); // 3 
// 使用自定义迭代器初始化集合
const s2 = new Set({ 
 [Symbol.iterator]: function*() { 
 yield "val1"; 
 yield "val2"; 
 yield "val3"; 
 } 
}); 
alert(s2.size); // 3 
```

初始化之后，可以使用 `add()`增加值，使用 `has()`查询，通过 `size` 取得元素数量，以及使用 `delete()` 和 `clear()`删除元素：

```js
const s = new Set(); 
alert(s.has("Matt")); // false 
alert(s.size); // 0 
s.add("Matt") 
 .add("Frisbie"); 
alert(s.has("Matt")); // true 
alert(s.size); // 2 
s.delete("Matt"); 
alert(s.has("Matt")); // false 
alert(s.has("Frisbie")); // true 
alert(s.size); // 1 
s.clear(); // 销毁集合实例中的所有值
alert(s.has("Matt")); // false 
alert(s.has("Frisbie")); // false 
alert(s.size); // 0 
```

`add()`返回集合的实例，所以可以将多个添加操作连缀起来，包括初始化：

```js
const s = new Set().add("val1"); 
s.add("val2") 
 .add("val3"); 
alert(s.size); // 3
```



与 `Map` 类似，`Set` 可以包含任何 `JavaScript` 数据类型作为值。集合也使用 `SameValueZero` 操作 （`ECMAScript` 内部定义，无法在语言中使用），基本上相当于使用严格对象相等的标准来检查值的匹配性。



```js
const s = new Set(); 
const functionVal = function() {}; 
const symbolVal = Symbol(); 
const objectVal = new Object(); 
s.add(functionVal); 
s.add(symbolVal); 
s.add(objectVal); 
alert(s.has(functionVal)); // true 
alert(s.has(symbolVal)); // true 
alert(s.has(objectVal)); // true 
// SameValueZero 检查意味着独立的实例不会冲突
alert(s.has(function() {})); // false
```



与严格相等一样，用作值的对象和其他“集合”类型在自己的内容或属性被修改时也不会改变：

```js
const s = new Set(); 
const objVal = {}, 
 arrVal = []; 
s.add(objVal); 
s.add(arrVal); 
objVal.bar = "bar"; 
arrVal.push("bar"); 
alert(s.has(objVal)); // true 
alert(s.has(arrVal)); // true 
```

`add()`和 `delete()`操作是幂等的。`delete()`返回一个布尔值，表示集合中是否存在要删除的值：

```js
const s = new Set(); 
s.add('foo'); 
alert(s.size); // 1 
s.add('foo'); 
alert(s.size); // 1 
// 集合里有这个值
alert(s.delete('foo')); // true 
// 集合里没有这个值
alert(s.delete('foo')); // false 
```



## 顺序与迭代

`Set` 会维护值插入时的顺序，因此支持按顺序迭代。 

集合实例可以提供一个迭代器（Iterator），能以插入顺序生成集合内容。可以通过 `values()`方法及其别名方法 `keys()`（或者 `Symbol.iterator` 属性，它引用 `values()`）取得这个迭代器：

```js
const s = new Set(["val1", "val2", "val3"]); 
alert(s.values === s[Symbol.iterator]); // true 
alert(s.keys === s[Symbol.iterator]); // true 
for (let value of s.values()) { 
 alert(value); 
} 
// val1 
// val2 
// val3 
for (let value of s[Symbol.iterator]()) { 
 alert(value); 
} 
// val1 
// val2 
// val3 
```

因为 `values()`是默认迭代器，所以可以直接对集合实例使用扩展操作，把集合转换为数组：

```js
const s = new Set(["val1", "val2", "val3"]); 
console.log([...s]); // ["val1", "val2", "val3"]
```

集合的 `entries()`方法返回一个迭代器，可以按照插入顺序产生包含两个元素的数组，这两个元素是集合中每个值的重复出现：

```js
const s = new Set(["val1", "val2", "val3"]); 
for (let pair of s.entries()) { 
 console.log(pair); 
} 
// ["val1", "val1"] 
// ["val2", "val2"] 
// ["val3", "val3"] 
```

如果不使用迭代器，而是使用回调方式，则可以调用集合的 `forEach()`方法并传入回调，依次迭代每个键/值对。传入的回调接收可选的第二个参数，这个参数用于重写回调内部 `this`的值：

```js
const s = new Set(["val1", "val2", "val3"]); 
s.forEach((val, dupVal) => alert(`${val} -> ${dupVal}`)); 
// val1 -> val1 
// val2 -> val2 
// val3 -> val3
```

修改集合中值的属性不会影响其作为集合值的身份：

```js
const s1 = new Set(["val1"]); 
// 字符串原始值作为值不会被修改
for (let value of s1.values()) { 
 value = "newVal"; 
 alert(value); // newVal 
 alert(s1.has("val1")); // true 
} 
const valObj = {id: 1}; 
const s2 = new Set([valObj]); 
// 修改值对象的属性，但对象仍然存在于集合中
for (let value of s2.values()) { 
 value.id = "newVal"; 
 alert(value); // {id: "newVal"} 
 alert(s2.has(valObj)); // true 
} 
alert(valObj); // {id: "newVal"}
```



## 定义正式集合操作

从各方面来看，`Set` 跟 `Map` 都很相似，只是 `API` 稍有调整。唯一需要强调的就是集合的 `API`对自 身的简单操作。很多开发者都喜欢使用 `Set` 操作，但需要手动实现：或者是子类化 `Set`，或者是定义一 个实用函数库。要把两种方式合二为一，可以在子类上实现静态方法，然后在实例方法中使用这些静态 方法。在实现这些操作时，需要考虑几个地方。

1. 某些 Set 操作是有关联性的，因此最好让实现的方法能支持处理任意多个集合实例。
2. Set 保留插入顺序，所有方法返回的集合必须保证顺序。
3. 尽可能高效地使用内存。扩展操作符的语法很简洁，但尽可能避免集合和数组间的相互转换能 够节省对象初始化成本。
4. 不要修改已有的集合实例。union(a, b)或 a.union(b)应该返回包含结果的新集合实例。



```js
class XSet extends Set { 
 union(...sets) { 
 	return XSet.union(this, ...sets) 
 } 
 intersection(...sets) { 
 	return XSet.intersection(this, ...sets); 
 } 
 difference(set) { 
 	return XSet.difference(this, set); 
 } 
 symmetricDifference(set) { 
 	return XSet.symmetricDifference(this, set); 
 } 
 cartesianProduct(set) { 
 	return XSet.cartesianProduct(this, set); 
 } 
 powerSet() { 
 	return XSet.powerSet(this); 
 } 
 // 返回两个或更多集合的并集
 static union(a, ...bSets) { 
     const unionSet = new XSet(a); 
     for (const b of bSets) { 
         for (const bValue of b) { 
         	unionSet.add(bValue); 
         } 
     } 
     return unionSet; 
 } 
 // 返回两个或更多集合的交集
 static intersection(a, ...bSets) { 
     const intersectionSet = new XSet(a); 
     for (const aValue of intersectionSet) { 
         for (const b of bSets) { 
             if (!b.has(aValue)) { 
             	intersectionSet.delete(aValue); 
         	 } 
         } 
     } 
     return intersectionSet; 
 } 
 // 返回两个集合的差集
 static difference(a, b) { 
     const differenceSet = new XSet(a); 
     for (const bValue of b) { 
         if (a.has(bValue)) { 
         	differenceSet.delete(bValue); 
         } 
     } 
     return differenceSet; 
 } 
 // 返回两个集合的对称差集
 static symmetricDifference(a, b) { 
     // 按照定义，对称差集可以表达为
     return a.union(b).difference(a.intersection(b)); 
 } 
 // 返回两个集合（数组对形式）的笛卡儿积
 // 必须返回数组集合，因为笛卡儿积可能包含相同值的对
 static cartesianProduct(a, b) { 
     const cartesianProductSet = new XSet(); 
     for (const aValue of a) { 
         for (const bValue of b) { 
            cartesianProductSet.add([aValue, bValue]); 
         } 
 	 } 
     return cartesianProductSet; 
 } 
 // 返回一个集合的幂集
 static powerSet(a) { 
     const powerSet = new XSet().add(new XSet()); 
     for (const aValue of a) {
         for (const set of new XSet(powerSet)) { 
         	powerSet.add(new XSet(set).add(aValue)); 
         } 
     } 
     return powerSet; 
 } 
} 
```



## WeakSet

`ES6`新增的“弱集合”（WeakSet）是一种新的集合类型，为这门语言带来了集合数据结构。WeakSet是Set的“兄弟”类型，其API也是Set的子集。WeakSet中的“weak”，描述的是JavaScript垃圾回收程序对待“弱集合”中值的方式。



### 基本API

可以使用new关键字实例化一个空的`WeakSet`：

```js
const ws = new WeakSet();
```

 弱集合中的值只能是Object或者继承自Object的类型，尝试使用非对象设置值会抛出TypeError。

如果想在初始化时填充弱集合，则构造函数可以接收一个可迭代对象，其中需要包含有效的值。可迭代对象中的每个值都会按照迭代顺序插入到新实例中：

```js
const val1 = {id: 1}, 
 val2 = {id: 2}, 
 val3 = {id: 3}; 
// 使用数组初始化弱集合
const ws1 = new WeakSet([val1, val2, val3]); 
alert(ws1.has(val1)); // true 
alert(ws1.has(val2)); // true 
alert(ws1.has(val3)); // true 
// 初始化是全有或全无的操作
// 只要有一个值无效就会抛出错误，导致整个初始化失败
const ws2 = new WeakSet([val1, "BADVAL", val3]); 
// TypeError: Invalid value used in WeakSet 
typeof ws2; 
// ReferenceError: ws2 is not defined 
// 原始值可以先包装成对象再用作值
const stringVal = new String("val1"); 
const ws3 = new WeakSet([stringVal]); 
alert(ws3.has(stringVal)); // true
```



初始化之后可以使用add()再添加新值，可以使用 has()查询，还可以使用 delete()删除：

```js
const ws = new WeakSet(); 
const val1 = {id: 1}, 
 val2 = {id: 2}; 
alert(ws.has(val1)); // false 
ws.add(val1) 
 .add(val2); 
alert(ws.has(val1)); // true 
alert(ws.has(val2)); // true 
ws.delete(val1); // 只删除这一个值
alert(ws.has(val1)); // false 
alert(ws.has(val2)); // true 
```



add()方法返回弱集合实例，因此可以把多个操作连缀起来，包括初始化声明：

```js
const val1 = {id: 1}, 
 val2 = {id: 2}, 
 val3 = {id: 3}; 
const ws = new WeakSet().add(val1); 
ws.add(val2) 
 .add(val3); 
alert(ws.has(val1)); // true 
alert(ws.has(val2)); // true 
alert(ws.has(val3)); // true
```



### 弱值

WeakSet 中“weak”表示弱集合的值是“弱弱地拿着”的。意思就是，**这些值不属于正式的引用， 不会阻止垃圾回收**。

```js
const ws = new WeakSet(); 
ws.add({}); 
```



`add()`方法初始化了一个新对象，并将它用作一个值。因为没有指向这个对象的其他引用，所以当 这行代码执行完成后，这个对象值就会被当作垃圾回收。然后，这个值就从弱集合中消失了，使其成为 一个空集合。

再看一个稍微不同的例子：

```js
const ws = new WeakSet(); 
const container = { 
 val: {} 
}; 
ws.add(container.val); 
function removeReference() { 
 container.val = null; 
} 
```

这一次，`container` 对象维护着一个对弱集合值的引用，因此这个对象值不会成为垃圾回收的目标。不过，如果调用了 `removeReference()`，就会摧毁值对象的最后一个引用，垃圾回收程序就可以把这个值清理掉。



### 不可迭代值

因为 WeakSet 中的值任何时候都可能被销毁，所以没必要提供迭代其值的能力。当然，也用不着 像 `clear()`这样一次性销毁所有值的方法。WeakSet 确实没有这个方法。因为不可能迭代，所以也不可能在不知道对象引用的情况下从弱集合中取得值。即便代码可以访问 WeakSet 实例，也没办法看到其中的内容。 

**WeakSet 之所以限制只能用对象作为值，是为了保证只有通过值对象的引用才能取得值。如果允许原始值，那就没办法区分初始化时使用的字符串字面量和初始化之后使用的一个相等的字符串了**。



### 使用弱集合

相比于 WeakMap 实例，WeakSet 实例的用处没有那么大。不过，弱集合在给对象打标签时还是有价值的。



来看下面的例子，这里使用了一个普通 Set：

```js
const disabledElements = new Set(); 
const loginButton = document.querySelector('#login'); 
// 通过加入对应集合，给这个节点打上“禁用”标签
disabledElements.add(loginButton); 
```



这样，通过查询元素在不在 `disabledElements` 中，就可以知道它是不是被禁用了。不过，假如 元素从 DOM 树中被删除了，它的引用却仍然保存在 Set 中，因此垃圾回收程序也不能回收它。



为了让垃圾回收程序回收元素的内存，可以在这里使用 WeakSet：

```js
const disabledElements = new WeakSet(); 
const loginButton = document.querySelector('#login');
// 通过加入对应集合，给这个节点打上“禁用”标签
disabledElements.add(loginButton);
```

这样，只要 WeakSet 中任何元素从 DOM 树中被删除，垃圾回收程序就可以忽略其存在，而立即释放其内存（假设没有其他地方引用这个对象）。



## 迭代与扩展操作

ECMAScript 6 新增的迭代器和扩展操作符对集合引用类型特别有用。这些新特性让集合类型之间相互操作、复制和修改变得异常方便。



有 4 种原生集合类型定义了默认迭代器：

1. Array
2. 所有定型数组
3. Map
4. Set



很简单，这意味着上述所有类型都支持顺序迭代，都可以传入 for-of 循环：

```js
let iterableThings = [ 
 Array.of(1, 2), 
 typedArr = Int16Array.of(3, 4), 
 new Map([[5, 6], [7, 8]]), 
 new Set([9, 10]) 
]; 
for (const iterableThing of iterableThings) { 
 for (const x of iterableThing) { 
 console.log(x); 
 } 
} 
// 1 
// 2 
// 3 
// 4 
// [5, 6] 
// [7, 8] 
// 9 
// 10 
```



这也意味着所有这些类型都兼容扩展操作符。扩展操作符在对可迭代对象执行浅复制时特别有用， 只需简单的语法就可以复制整个对象：

```js
let arr1 = [1, 2, 3]; 
let arr2 = [...arr1]; 
console.log(arr1); // [1, 2, 3] 
console.log(arr2); // [1, 2, 3] 
console.log(arr1 === arr2); // false
```



对于期待可迭代对象的构造函数，只要传入一个可迭代对象就可以实现复制：

```js
let map1 = new Map([[1, 2], [3, 4]]); 
let map2 = new Map(map1); 
console.log(map1); // Map {1 => 2, 3 => 4} 
console.log(map2); // Map {1 => 2, 3 => 4}
```



当然，也可以构建数组的部分元素：

```js
let arr1 = [1, 2, 3]; 
let arr2 = [0, ...arr1, 4, 5]; 
console.log(arr2); // [0, 1, 2, 3, 4, 5]
```



浅复制意味着只会复制对象引用：

```js
let arr1 = [{}]; 
let arr2 = [...arr1]; 
arr1[0].foo = 'bar'; 
console.log(arr2[0]); // { foo: 'bar' } 
```



上面的这些类型都支持多种构建方法，比如 `Array.of()`和 `Array.from()`静态方法。在与扩展操作符一起使用时，可以非常方便地实现互操作：

```js
let arr1 = [1, 2, 3]; 
// 把数组复制到定型数组
let typedArr1 = Int16Array.of(...arr1); 
let typedArr2 = Int16Array.from(arr1); 
console.log(typedArr1); // Int16Array [1, 2, 3] 
console.log(typedArr2); // Int16Array [1, 2, 3] 
// 把数组复制到映射
let map = new Map(arr1.map((x) => [x, 'val' + x])); 
console.log(map); // Map {1 => 'val 1', 2 => 'val 2', 3 => 'val 3'} 
// 把数组复制到集合
let set = new Set(typedArr2); 
console.log(set); // Set {1, 2, 3} 
// 把集合复制回数组
let arr2 = [...set]; 
console.log(arr2); // [1, 2, 3] 
```

