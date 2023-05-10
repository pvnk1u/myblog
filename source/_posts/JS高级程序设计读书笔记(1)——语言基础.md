---
title: "JavaScript高级程序设计读书笔记(1)——语言基础" 
date: 2022-07-18T12:32:39+08:00
draft: false
tags: ["web","javascript"]
categories:
  - "web"
  - "javascript"
toc: true
---



# 语法

1. 区分大小写

   `ECMAScript` 中一切都区分大小写。无论是变量、函数名还是操作符，都区分大小写。

2. 标识符

   所谓标识符，就是变量、函数、属性或函数参数的名称。标识符可以由一或多个下列字符组成：

   - 第一个字符必须是一个字母、下划线（_）或美元符号（$）；
   - 剩下的其他字符可以是字母、下划线、美元符号或数字

3. 严格模式

4. `ECMAScript 5` 增加了严格模式（`strict mode`）的概念。严格模式是一种不同的 `JavaScript` 解析和执 行模型，`ECMAScript 3` 的一些不规范写法在这种模式下会被处理，对于不安全的活动将抛出错误。要对整个脚本启用严格模式，在脚本开头加上这一行：

   ```js
   "use strict"; 
   ```

   虽然看起来像个没有赋值给任何变量的字符串，但它其实是一个预处理指令。任何支持的 `JavaScript` 引擎看到它都会切换到严格模式。选择这种语法形式的目的是不破坏`ECMAScript 3` 语法。 

   也可以单独指定一个函数在严格模式下执行，只要把这个预处理指令放到函数体开头即可：

   ```js
   function doSomething() { 
    "use strict"; 
    // 函数体 
   } 
   ```

5. 语句

   `ECMAScript` 中的语句以分号结尾。省略分号意味着由解析器确定语句在哪里结尾。

   即使语句末尾的分号不是必需的，也应该加上。记着加分号有助于防止省略造成的问题，比如可以 避免输入内容不完整。此外，加分号也便于开发者通过删除空行来压缩代码（如果没有结尾的分号，只 删除空行，则会导致语法错误）。加分号也有助于在某些情况下提升性能，因为解析器会尝试在合适的 位置补上分号以纠正语法错误。

# 变量

`ECMAScript` 变量是松散类型的，意思是变量可以用于保存任何类型的数据。每个变量只不过是一 个用于保存任意值的命名占位符。有 3 个关键字可以声明变量：`var`、`const` 和 `let`。其中，`var` 在 `ECMAScript` 的所有版本中都可以使用，而 `const` 和 `let` 只能在 `ECMAScript 6` 及更晚的版本中使用。



## var关键字

```js
var message; 
```

这行代码定义了一个名为 message 的变量，可以用它保存任何类型的值。（不初始化的情况下，变 量会保存一个特殊值 undefined）`ECMAScript` 实现变量初始化，因 此可以同时定义变量并设置它的值：

```js
var message = "hi"; 
```

这里，message 被定义为一个保存字符串值 hi 的变量。像这样初始化变量不会将它标识为字符串 类型，只是一个简单的赋值而已。随后，不仅可以改变保存的值，也可以改变值的类型：

```js
var message = "hi"; 
message = 100; // 合法，但不推荐
```

1. var声明作用域

   关键的问题在于，使用 var 操作符定义的变量会成为包含它的函数的局部变量。比如，使用 var 在一个函数内部定义一个变量，就意味着该变量将在函数退出时被销毁：

   ```js
   function test() { 
    var message = "hi"; // 局部变量
   } 
   test(); 
   console.log(message); // 出错！
   ```

   这里，message 变量是在函数内部使用 var 定义的。函数叫 test()，调用它会创建这个变量并给 它赋值。调用之后变量随即被销毁，因此示例中的最后一行会导致错误。不过，在**函数内定义变量时省略 var 操作符，可以创建一个全局变量**：

   ```js
   function test() { 
    message = "hi"; // 全局变量，不推荐
   } 
   test(); 
   console.log(message); // "hi" 
   ```

   **去掉之前的 var 操作符之后，message 就变成了全局变量。**只要调用一次函数 test()，就会定义这个变量，并且可以在函数外部访问到。

   在严格模式下，如果像这样给未声明的变量赋值，则会导致抛出 `ReferenceError`。

   如果需要定义多个变量，可以在一条语句中用逗号分隔每个变量（及可选的初始化）：

   ```js
   var message = "hi", 
    found = false, 
    age = 29;
   ```

2. var声明提升

   使用 var 时，下面的代码不会报错。这是因为使用这个关键字声明的变量会自动提升到函数作用域 顶部：

   ```js
   function foo() { 
    console.log(age); 
    var age = 26; 
   } 
   foo(); // undefined
   ```

   之所以不会报错，是因为 `ECMAScript` 运行时把它看成等价于如下代码：

   ```javascript
   function foo() { 
    var age; 
    console.log(age); 
    age = 26; 
   } 
   foo(); // undefined
   ```

   这就是所谓的“提升”（hoist），也就是把所有变量声明都拉到函数作用域的顶部。此外，反复多次 使用 var 声明同一个变量也没有问题：

   ```js
   function foo() { 
    var age = 16; 
    var age = 26; 
    var age = 36; 
    console.log(age); 
   } 
   foo(); // 36
   ```

## let声明

let 跟 var 的作用差不多，但有着非常重要的区别。最明显的区别是，**let 声明的范围是块作用域， 而 var 声明的范围是函数作用域。**

```js
if (true) { 
 var name = 'Matt'; 
 console.log(name); // Matt 
} 
console.log(name); // Matt
if (true) { 
 let age = 26; 
 console.log(age); // 26 
} 
console.log(age); // ReferenceError: age 没有定义
```

在这里，age 变量之所以不能在 if 块外部被引用，是因为它的作用域仅限于该块内部。**块作用域是函数作用域的子集，因此适用于 var 的作用域限制同样也适用于 let。**

let 也不允许同一个块作用域中出现冗余声明。这样会导致报错：

```js
if(true){
    var name = 'Matt';
    var name = 'Nick'; 
} 
if(true){
    let age = 1;
    let age = 2; // SyntaxError；标识符 age 已经声明过了
} 
```



## const声明

`const` 的行为与 `let` 基本相同，唯一一个重要的区别是用它声明变量时必须同时初始化变量，且 尝试修改 `const` 声明的变量会导致运行时错误。

```javascript
if(true){
	const age = 26;
	age = 36; // TypeError: 给常量赋值
}
if(true){
    // const 也不允许重复声明
	const name = 'Matt';
	const name = 'Nicholas'; // SyntaxError
}
// const 声明的作用域也是块
const name = 'Matt';
if (true) {
 const name = 'Nicholas';
}
console.log(name); // Matt 
```

**`const` 声明的限制只适用于它指向的变量的引用。换句话说，如果 `const` 变量引用的是一个对象， 那么修改这个对象内部的属性并不违反 `const` 的限制。**



```js
const person = {}; 
person.name = 'Matt'; // ok 
```

`JavaScript` 引擎会为 `for` 循环中的 `let` 声明分别创建独立的变量实例，虽然 `const` 变量跟 `let` 变 量很相似，但是不能用 `const` 来声明迭代变量（因为迭代变量会自增）：

```js
for (const i = 0; i < 10; ++i) {} // TypeError：给常量赋值
```

不过，如果只想用 `const` 声明一个不会被修改的 `for` 循环变量，那也是可以的。也就是说，每次迭代只是创建一个新变量。这对 `for-of` 和 `for-in` 循环特别有意义:

```js
let i = 0; 
for (const j = 7; i < 5; ++i) { 
 console.log(j); 
} 
// 7, 7, 7, 7, 7 
for (const key in {a: 1, b: 2}) { 
 console.log(key); 
} 
// a, b 
for (const value of [1,2,3,4,5]) { 
 console.log(value); 
} 
// 1, 2, 3, 4, 5
```



## 风格最佳实践

`ECMAScript` 6 增加 `let` 和 `const` 从客观上为这门语言更精确地声明作用域和语义提供了更好的支持。行为怪异的 `var` 所造成的各种问题，已经让 `JavaScript`社区为之苦恼了很多年。随着这两个新关键 字的出现，新的有助于提升代码质量的最佳实践也逐渐显现。

1. 不使用var

   有了 `let` 和 `const`，大多数开发者会发现自己不再需要 `var` 了。限制自己只使用 `let` 和 `const` 有助于提升代码质量，因为变量有了明确的作用域、声明位置，以及不变的值。

2. const优先，let次之

   使用 `const`声明可以让浏览器运行时强制保持变量不变，也可以让静态代码分析工具提前发现不合法的赋值操作。因此，很多开发者认为应该优先使用 `const` 来声明变量，只在提前知道未来会有修改时，再使用 `let`。这样可以让开发者更有信心地推断某些变量的值永远不会变，同时也能迅速发现因意外赋值导致的非预期行为。



# 数据类型

`ECMAScript` 有 6 种简单数据类型（也称为原始类型）：`Undefined`、`Null`、`Boolean`、`Number`、 `String` 和 `Symbol`。`Symbol`（符号）是 `ECMAScript` 6 新增的。还有一种复杂数据类型叫 `Object`（对 象）。`Object` 是一种无序名值对的集合。因为在 `ECMAScript` 中不能定义自己的数据类型，所有值都可以用上述 7 种数据类型之一来表示。只有 7 种数据类型似乎不足以表示全部数据。但 `ECMAScript` 的数据类型很灵活，一种数据类型可以当作多种数据类型来使用。

## typeof操作符

因为 `ECMAScript` 的类型系统是松散的，所以需要一种手段来确定任意变量的数据类型。`typeof` 操作符就是为此而生的。对一个值使用 `typeof`操作符会返回下列字符串之一：

1. "undefined"表示值未定义；
2. "boolean"表示值为布尔值；
3. "string"表示值为字符串；
4. "number"表示值为数值；
5. "object"表示值为对象（而不是函数）或 null；
6. "function"表示值为函数；
7. "symbol"表示值为符号。



下面是使用 `typeof` 操作符的例子：

```js
let message = "some string"; 
console.log(typeof message); // "string" 
console.log(typeof(message)); // "string" 
console.log(typeof 95); // "number"
```

注意`typeof`在某些情况下返回的结果可能会让人费解，但技术上讲还是正确的。比如，调用`typeof  null` 返回的是"object"。这是因为特殊值 `null` 被认为是一个对空对象的引用。



## Undefined类型

`Undefined` 类型只有一个值，就是特殊值 `undefined`。当使用 `var` 或 `let`声明了变量但没有初始化时，就相当于给变量赋予了 undefined 值：

```js
let message; 
console.log(message == undefined); // true
```

包含 undefined 值的变量跟未定义变量是有区别的。对未声明的变量，只能执行一个 有用的操作，就是对它调用 `typeof`。（对未声明的变量调用 delete 也不会报错，但这个操作没什么用， 实际上在严格模式下会抛出错误。）



在对未初始化的变量调用 `typeof` 时，返回的结果是"undefined"，但对未声明的变量调用它时， 返回的结果还是"undefined"，无论是声明还是未声明，`typeof `返回的都是字符串"undefined"。逻辑上讲这是对的，因为虽然 严格来讲这两个变量存在根本性差异，但它们都无法执行实际操作。



即使未初始化的变量会被自动赋予 `undefined` 值，但仍然建议在声明变量的同时进行初始化。这样，当 `typeof` 返回"undefined"时，就会知道那是因为给定的变量尚未声明，而不是声明了但未初始化。



undefined 是一个假值。因此，如果需要，可以用更简洁的方式检测它。不过要记住，也有很多其他可能的值同样是假值。所以一定要明确自己想检测的就是 undefined 这个字面值，而不仅仅是 假值。

```js
let message; // 这个变量被声明了，只是值为 undefined 
// age 没有声明 
if (message) { 
 // 这个块不会执行
} 
if (!message) { 
 // 这个块会执行
} 
if (age) { 
 // 这里会报错
} 
```



## Null类型

`Null` 类型同样只有一个值，即特殊值 `null`。逻辑上讲，`null` 值表示一个空对象指针，这也是给 `typeof` 传一个 `null` 会返回"object"的原因：

```js
let car = null; 
console.log(typeof car); // "object"
```

在定义将来要保存对象值的变量时，建议使用 `null` 来初始化，不要使用其他值。这样，只要检查 这个变量的值是不是 `null` 就可以知道这个变量是否在后来被重新赋予了一个对象的引用，比如：

```js
if (car != null) { 
 // car 是一个对象的引用
}
```

undefined 值是由 null 值派生而来的，因此 `ECMA-262` 将它们定义为表面上相等，如下面的例 子所示：

```js
console.log(null == undefined); // true 
```

即使 `null` 和 `undefined` 有关系，它们的用途也是完全不一样的。如前所述，永远不必显式地将 变量值设置为 `undefined`。但 `null` 不是这样的。任何时候，只要变量要保存对象，而当时又没有那个 对象可保存，就要用 `null` 来填充该变量。这样就可以保持 `null` 是空对象指针的语义，并进一步将其 与 `undefined` 区分开来。



`null` 是一个假值。因此，如果需要，可以用更简洁的方式检测它。不过要记住，也有很多其他可 能的值同样是假值。所以一定要明确自己想检测的就是 `null` 这个字面值，而不仅仅是假值。

```js
let message = null; 
let age; 
if (message) { 
 // 这个块不会执行
} 
if (!message) { 
 // 这个块会执行
}
if (age) { 
 // 这个块不会执行
} 
if (!age) { 
 // 这个块会执行
}
```



## Boolean类型

`Boolean`（布尔值）类型是 `ECMAScript` 中使用最频繁的类型之一，有两个字面值：true 和 false。 这两个布尔值不同于数值，因此 true 不等于 1，false 不等于 0。



布尔值字面量 `true` 和 `false` 是区分大小写的，因此 `True` 和 `False`（及其他大小混写形式） 是有效的标识符，但不是布尔值。



虽然布尔值只有两个，但所有其他 `ECMAScript` 类型的值都有相应布尔值的等价形式。要将一个其 他类型的值转换为布尔值，可以调用特定的 `Boolean()`转型函数：

```js
let message = "Hello world!"; 
let messageAsBoolean = Boolean(message); 
```

`Boolean()`转型函数可以在任意类型的数据上调用，而且始终返回一个布尔值。什么值能转换为 `true` 或 `false`的规则取决于数据类型和实际的值。下表总结了不同类型与布尔值之间的转换规则。



| 数据类型  | 转化为true值           | 转化为false的值 |
| --------- | ---------------------- | --------------- |
| Boolean   | true                   | false           |
| String    | 非空字符串             | ""(空字符串)    |
| Number    | 非零数值（包括无穷值） | 0、NaN          |
| Object    | 任意对象               | null            |
| Undefined | N/A(不存在)            | undefined       |



if 等流控制语句会自动执行其他类型值到布尔值的转换:

```js
let message = "Hello world!"; 
if (message) { 
 console.log("Value is true"); 
} 
```

`console.log` 会输出字符串"Value is true"，因为字符串 `message` 会被自动 转换为等价的布尔值 `true`。



## Number类型

`Number` 类型使用 `IEEE 754` 格式表示整 数和浮点值（在某些语言中也叫双精度值）。不同的数值类型相应地也有不同的数值字面量格式。

最基本的数值字面量格式是十进制整数，直接写出来即可：

```js
let intNum = 55; // 整数
```

整数也可以用八进制（以8 为基数）或十六进制（以16 为基数）字面量表示。对于八进制字面量，第一个数字必须是零（0），然后是相应的八进制数字（数值 0~7）。如果字面量中包含的数字超出了应有的范围，就会忽略前缀的零，后面的数字序列会被当成十进制数，如下所示：

```js
let octalNum1 = 070;  // 八进制的56 
let octalNum2 = 079;  // 无效的八进制值，当成79 处理 
let octalNum3 = 08;   // 无效的八进制值，当成8处理
```

八进制字面量在严格模式下是无效的，会导致`JavaScript` 引擎抛出语法错误。



要创建十六进制字面量，必须让真正的数值前缀 `0x`（区分大小写），然后是十六进制数字（0~9 以及A~F）。十六进制数字中的字母大小写均可。下面是几个例子：

```js
let hexNum1 = 0xA;   // 十六进制10 
let hexNum2 = 0x1f;  // 十六进制31
```

**使用八进制和十六进制格式创建的数值在所有数学操作中都被视为十进制数值。**



**由于JavaScript 保存数值的方式，实际中可能存在正零（+0）和负零（-0）。正零和负零在所有情况下都被认为是等同的。**



1. 浮点值

   要定义浮点值，数值中必须包含小数点，而且小数点后面必须至少有一个数字。虽然小数点前面不是必须有整数，但推荐加上。下面是几个例子：

   ```js
   let floatNum1 = 1.1; 
   let floatNum2 = 0.1; 
   let floatNum3 = .1;   // 有效，但不推荐
   ```

   因为**存储浮点值使用的内存空间是存储整数值的两倍**，所以`ECMAScript` 总是想方设法把值转换为整数。在小数点后面没有数字的情况下，数值就会变成整数。类似地，如果数值本身就是整数，只是小数点后面跟着0（如1.0），那它也会被转换为整数，如下例所示：

   ```js
   let floatNum1 = 1.;   // 小数点后面没有数字，当成整数1 处理 
   let floatNum2 = 10.0; // 小数点后面是零，当成整数10 处理
   ```

   对于非常大或非常小的数值，浮点值可以用科学记数法来表示。科学记数法用于表示一个应该乘以10 的给定次幂的数值。`ECMAScript` 中科学记数法的格式要求是一个数值（整数或浮点数）后跟一个大写或小写的字母e，再加上一个要乘的10 的多少次幂。比如：

   ```js
   let floatNum = 3.125e7; // 等于31250000
   ```

   在这个例子中，`floatNum` 等于`31250000`，只不过科学记数法显得更简洁。这种表示法实际上相当于说：“以3.125 作为系数，乘以10 的7 次幂。”

   科学记数法也可以用于表示非常小的数值，例如`0.000 000 000 000 000 03`。这个数值用科学记数法可以表示为`3e-17`。默认情况下，`ECMAScript` 会将小数点后至少包含6 个零的浮点值转换为科学记数法。

   

   **浮点值的精确度最高可达 17 位小数，但在算术计算中远不如整数精确**。例如，0.1 加 0.2 得到的不是0.3，而是 0.300 000 000 000 000 04。由于这种微小的舍入错误，导致很难测试特定的浮点值。比如下面的例子：

   ```js
   if (a + b == 0.3) {      // 别这么干！   
     console.log("You got 0.3."); 
   }
   ```

   这里检测两个数值之和是否等于0.3。如果两个数值分别是0.05 和0.25，或者 0.15 和0.15，那没问题。但如果是0.1 和0.2，如前所述，测试将失败。因此**永远不要测试某个特定的浮点值**。

   

   之所以存在这种舍入错误，是因为使用了`IEEE 754` 数值，这种错误并非`ECMAScript`所独有。其他使用相同格式的语言也有这个问题。

2. 值的范围

   由于内存的限制，`ECMAScript` 并不支持表示这个世界上的所有数值。`ECMAScript` 可以表示的最小数值保存在 `Number.MIN_VALUE` 中，这个值在多数浏览器中是 `5e-324`；可以表示的最大数值保存在`Number.MAX_VALUE` 中，这个值在多数浏览器中是`1.797 693 134 862 315 7e+308`。如果某个计算得到的数值结果超出了 `JavaScript` 可以表示的范围，那么这个数值会被自动转换为一个特殊的 `Infinity`（无穷）值。任何无法表示的负数以`-Infinity`（负无穷大）表示，任何无法表示的正数以`Infinity`（正无穷大）表示。

   

   如果计算返回正 `Infinity` 或负 `Infinity`，则该值将不能再进一步用于任何计算。这是因为`Infinity` 没有可用于计算的数值表示形式。要确定一个值是不是有限大（即介于 `JavaScript` 能表示的最小值和最大值之间），可以使用`isFinite()`函数，如下所示：

   ```js
   let result = Number.MAX_VALUE + Number.MAX_VALUE; 
   console.log(isFinite(result));  // false
   ```

   使用 `Number.NEGATIVE_INFINITY` 和 `Number.POSITIVE_INFINITY` 也可以获取正、负`Infinity`。没错，这两个属性包含的值分别就是`-Infinity` 和`Infinity`。

3. NaN

   有一个特殊的数值叫`NaN`，意思是“不是数值”（`Not a Number`），用于表示本来要返回数值的操作失败了（而不是抛出错误）。比如，用 0 除任意数值在其他语言中通常都会导致错误，从而中止代码执行。但在`ECMAScript` 中，0、+0 或-0 相除会返回`NaN`：

   ```js
   console.log(0/0);    // NaN 
   console.log(-0/+0);  // NaN 
   ```

   如果分子是非0 值，分母是有符号0 或无符号0，则会返回`Infinity` 或`-Infinity`：

   ```js
   console.log(5/0);   // Infinity 
   console.log(5/-0);  // -Infinity
   ```

   `NaN` 有几个独特的属性。首先，任何涉及`NaN` 的操作始终返回`NaN`（如`NaN/10`），在连续多步计算时这可能是个问题。其次，`NaN` 不等于包括`NaN` 在内的任何值。例如，下面的比较操作会返回false：

   ```js
   console.log(NaN == NaN); // false 
   ```

   为此，`ECMAScript` 提供了 `isNaN()`函数。该函数接收一个参数，可以是任意数据类型，然后判断这个参数是否“不是数值”。把一个值传给`isNaN()`后，该函数会尝试把它转换为数值。某些非数值的值可以直接转换成数值，如字符串"10"或布尔值。任何不能转换为数值的值都会导致这个函数返回`true`。举例如下：

   ```js
   console.log(isNaN(NaN));     // true 
   console.log(isNaN(10));      // false，10 是数值 
   console.log(isNaN("10"));    // false，可以转换为数值10 
   console.log(isNaN("blue"));  // true，不可以转换为数值 
   console.log(isNaN(true));    // false，可以转换为数值1
   ```

   虽然不常见，但`isNaN()`可以用于测试对象。此时，首先会调用对象的`valueOf()`方法，然后再确定返回的值是否可以转换为数值。如果不能，再调用`toString()`方法，并测试其返回值。这通常是`ECMAScript`内置函数和操作符的工作方式。

4. 数值转换

   有3 个函数可以将非数值转换为数值：`Number()`、`parseInt()`和`parseFloat()`。`Number()`是转型函数，可用于任何数据类型。**后两个函数主要用于将字符串转换为数值。**对于同样的参数，这3 个函数执行的操作也不同。

   `Number()`函数基于如下规则进行转换：

   - 布尔值，true转换为1，false转换为0
   - 数值，直接返回
   - null，返回0
   - undefined，返回NaN
   - 字符串，应用以下规则
     - 如果字符串包含数值字符，包括数值字符前面带加、减号的情况，则转换为一个十进制数值。因此，Number("1")返回1，Number("123")返回123，Number("011")返回11（忽略前面的0）。
     - 如果字符串包含有效的浮点值格式如”1.1“，则会转换为相应的浮点值（同样，忽略前面的零）。
     - 如果字符串包含有效的十六进制格式如"0xf"，则会转换为与该十六进制值对应的十进制整数值。
     - 如果是空字符串（不包含字符），则返回0。
     - 如果字符串包含除上述情况之外的其他字符，则返回`NaN`。
   - 对象，调用 `valueOf()`方法，并按照上述规则转换返回的值。如果转换结果是 `NaN`，则调用`toString()`方法，再按照转换字符串的规则转换。

   ```js
   let num1 = Number("Hello world!");  // NaN 
   let num2 = Number("");              // 0 
   let num3 = Number("000011");        // 11 
   let num4 = Number(true);            // 1
   ```

   考虑到用`Number()`函数转换字符串时相对复杂且有点反常规，通常在需要得到整数时可以优先使用 `parseInt()`函数。`parseInt()`函数更专注于字符串是否包含数值模式。字符串最前面的空格会被忽略，从第一个非空格字符开始转换。如果第一个字符不是数值字符、加号或减号，`parseInt()`立即返回`NaN`。这意味着空字符串也会返回`NaN`（这一点跟`Number()`不一样，它返回0）。如果第一个字符是数值字符、加号或减号，则继续依次检测每个字符，直到字符串末尾，或碰到非数值字符。比如，"1234blue"会被转换为1234，因为"blue"会被完全忽略。类似地，"22.5"会被转换为22，因为小数点不是有效的整数字符。

   

   假设字符串中的第一个字符是数值字符，`parseInt()`函数也能识别不同的整数格式（十进制、八进制、十六进制）。换句话说，如果字符串以"0x"开头，就会被解释为十六进制整数。如果字符串以"0"开头，且紧跟着数值字符，在非严格模式下会被某些实现解释为八进制整数。

   ```js
   let num1 = parseInt("1234blue");  // 1234 
   let num2 = parseInt("");          // NaN 
   let num3 = parseInt("0xA");       // 10，解释为十六进制整数 
   
   let num4 = parseInt(22.5);        // 22 
   let num5 = parseInt("70");        // 70，解释为十进制值 
   let num6 = parseInt("0xf");       // 15，解释为十六进制整数
   ```

   不同的数值格式很容易混淆，因此 `parseInt()`也接收第二个参数，用于指定底数（进制数）。如果知道要解析的值是十六进制，那么可以传入16 作为第二个参数，以便正确解析：

   ```js
   let num = parseInt("0xAF", 16); // 175
   ```

   `parseFloat()`函数的工作方式跟`parseInt()`函数类似，都是从位置0 开始检测每个字符。同样，它也是解析到字符串末尾或者解析到一个无效的浮点数值字符为止。这意味着第一次出现的小数点是有效的，但第二次出现的小数点就无效了，此时字符串的剩余字符都会被忽略。因此，"22.34.5"将转换成22.34。

   `parseFloat()`函数的另一个不同之处在于，它始终忽略字符串开头的零。这个函数能识别前面讨论的所有浮点格式，以及十进制格式（开头的零始终被忽略）。十六进制数值始终会返回 0。因为`parseFloat()`只解析十进制值，因此不能指定底数。最后，如果字符串表示整数（没有小数点或者小数点后面只有一个零），则`parseFloat()`返回整数。下面是几个示例： 

   ```js
   let num1 = parseFloat("1234blue");  // 1234，按整数解析 
   let num2 = parseFloat("0xA");       // 0 
   let num3 = parseFloat("22.5");      // 22.5 
   let num4 = parseFloat("22.34.5");   // 22.34 
   
   let num5 = parseFloat("0908.5");    // 908.5 
   let num6 = parseFloat("3.125e7");   // 31250000
   ```

   

## String类型

`String`（字符串）数据类型表示零或多个 16 位 Unicode 字符序列。字符串可以使用双引号（"）、单引号（'）或反引号（`）标示。

- 字符串的特点

  `ECMAScript` 中的字符串是不可变的（`immutable`），意思是一旦创建，它们的值就不能变了。要修改某个变量中的字符串值，必须先销毁原始的字符串，然后将包含新值的另一个字符串保存到该变量，如下所示：

  ```js
  let lang = "Java"; 
  lang = lang + "Script";
  ```

  这里，变量`lang` 一开始包含字符串"Java"。紧接着，`lang` 被重新定义为包含"Java"和"Script"的组合，也就是"JavaScript"。整个过程首先会分配一个足够容纳 10 个字符的空间，然后填充上"Java"和"Script"。最后销毁原始的字符串"Java"和字符串"Script"，因为这两个字符串都没有用了。

- 转换为字符串

  有两种方式把一个值转换为字符串。首先是使用几乎所有值都有的`toString()`方法。这个方法唯一的用途就是返回当前值的字符串等价物。比如：

  ```js
  let age = 11; 
  let ageAsString = age.toString();      // 字符串"11" 
  let found = true; 
  
  let foundAsString = found.toString();  // 字符串"true"
  ```

  `toString()`方法可见于数值、布尔值、对象和字符串值。（没错，字符串值也有`toString()`方法，该方法只是简单地返回自身的一个副本。）`null` 和`undefined `值没有`toString()`方法。

  

  多数情况下，`toString()`不接收任何参数。不过，在对数值调用这个方法时，`toString()`可以接收一个底数参数，即以什么底数来输出数值的字符串表示。默认情况下，`toString()`返回数值的十进制字符串表示。而通过传入参数，可以得到数值的二进制、八进制、十六进制，或者其他任何有效基数的字符串表示，比如：

  ```js
  let num = 10; 
  console.log(num.toString());     // "10" 
  console.log(num.toString(2));    // "1010" 
  console.log(num.toString(8));    // "12" 
  
  console.log(num.toString(10));   // "10" 
  
  console.log(num.toString(16));   // "a" 
  ```

  如果不确定一个值是不是`null` 或`undefined`，可以使用 `String()`转型函数，它始终会返回表示相应类型值的字符串。`String()`函数遵循如下规则。

  - 如果值有`toString()`方法，则调用该方法（不传参数）并返回结果
  - 如果值是`null`，返回"null"
  - 如果值是`undefined`，返回"undefined"

  用加号操作符给一个值加上一个空字符串""也可以将其转换为字符串。

- 模板字面量

  `ECMAScript` 6 新增了使用模板字面量定义字符串的能力。与使用单引号或双引号不同，模板字面量保留换行字符，可以跨行定义字符串：

  ```js
  let myMultiLineString = 'first line\nsecond line'; 
  let myMultiLineTemplateLiteral = `first line 
  second line`; 
   
  console.log(myMultiLineString); 
  
  // first line 
  // second line" 
   
  
  console.log(myMultiLineTemplateLiteral); 
  // first line 
  // second line 
   
  
  console.log(myMultiLineString === myMultiLinetemplateLiteral); // true
  ```

  顾名思义，模板字面量在定义模板时特别有用，比如下面这个HTML 模板：

  ```js
  let pageHTML = `  
  
  <div> 
    <a href="#"> 
      <span>Jake</span> 
  
    </a> 
  
  </div>`;
  ```

- 字符串插值

  模板字面量最常用的一个特性是支持字符串插值，也就是可以在一个连续定义中插入一个或多个值。技术上讲，模板字面量不是字符串，而是一种特殊的 `JavaScript` 句法表达式，只不过求值后得到的是字符串。模板字面量在定义时立即求值并转换为字符串实例，任何插入的变量也会从它们最接近的作用域中取值。

  字符串插值通过在`${}`中使用一个`JavaScript` 表达式实现：

  ```js
  let value = 5;  
  let exponent = 'second'; 
  // 以前，字符串插值是这样实现的： 
  let interpolatedString = 
    value + ' to the ' + exponent + ' power is ' + (value * value); 
  
   
  // 现在，可以用模板字面量这样实现： 
  let interpolatedTemplateLiteral = 
  
    `${ value } to the ${ exponent } power is ${ value * value }`; 
   
  console.log(interpolatedString);           // 5 to the second power is 25 
  console.log(interpolatedTemplateLiteral);  // 5 to the second power is 25
  ```

  所有插入的值都会使用 `toString()`强制转型为字符串，而且任何 `JavaScript` 表达式都可以用于插值。嵌套的模板字符串无须转义：

  ```js
  console.log(`Hello, ${ `World` }!`);  // Hello, World! 
  ```

  将表达式转换为字符串时会调用`toString()`：

  ```js
  let foo = { toString: () => 'World' }; 
  console.log(`Hello, ${ foo }!`);      // Hello, World!
  ```

  在插值表达式中可以调用函数和方法：

  ```js
  function capitalize(word) {  
  
    return `${ word[0].toUpperCase() }${ word.slice(1) }`; 
  } 
  console.log(`${ capitalize('hello') }, ${ capitalize('world') }!`); // Hello, World! 
  ```

  此外，模板也可以插入自己之前的值：

  ```js
  let value = ''; 
  function append() { 
    value = `${value}abc` 
  
    console.log(value); 
  } 
  append();  // abc 
  
  append();  // abcabc 
  
  append();  // abcabcabc
  ```

## Symbol类型

`Symbol`（符号）是`ECMAScript` 6 新增的数据类型。符号是原始值，且符号实例是唯一、不可变的。符号的用途是确保对象属性使用唯一标识符，不会发生属性冲突的危险。

尽管听起来跟私有属性有点类似，但符号并不是为了提供私有属性的行为才增加的（尤其是因为`Object` API 提供了方法，可以更方便地发现符号属性）。相反，符号就是用来创建唯一记号，进而用作非字符串形式的对象属性。

1. 符号的基本用法

   符号需要使用`Symbol()`函数初始化。因为符号本身是原始类型，所以`typeof` 操作符对符号返回`symbol`。

   ```js
   let sym = Symbol(); 
   
   console.log(typeof sym); // symbol
   ```

   调用`Symbol()`函数时，也可以传入一个字符串参数作为对符号的描述（`description`），将来可以通过这个字符串来调试代码。但是，这个字符串参数与符号定义或标识完全无关。

   最重要的是，`Symbol()`函数不能与`new` 关键字一起作为构造函数使用。这样做是为了避免创建符号包装对象，像使用`Boolean`、`String` 或`Number`那样，它们都支持构造函数且可用于初始化包含原始值的包装对象：

   ```js
   let myBoolean = new Boolean(); 
   console.log(typeof myBoolean); // "object" 
    
   let myString = new String(); 
   console.log(typeof myString);  // "object" 
   
    
   let myNumber = new Number(); 
   console.log(typeof myNumber);  // "object"
   let mySymbol = new Symbol(); // TypeError: Symbol is not a constructor
   ```

   如果确实想使用符号包装对象，可以借用`Object()`函数：

   ```js
   let mySymbol = Symbol(); 
   
   let myWrappedSymbol = Object(mySymbol); 
   
   console.log(typeof myWrappedSymbol);   // "object"
   ```

2. 使用全局符号注册表

   **如果运行时的不同部分需要共享和重用符号实例，那么可以用一个字符串作为键，在全局符号注册表中创建并重用符号**。

   为此，需要使用`Symbol.for()`方法：

   ```js
   let fooGlobalSymbol = Symbol.for('foo'); 
   
   console.log(typeof fooGlobalSymbol); // symbol 
   ```

   `Symbol.for()`对每个字符串键都执行幂等操作。第一次使用某个字符串调用时，它会检查全局运行时注册表，发现不存在对应的符号，于是就会生成一个新符号实例并添加到注册表中。后续使用相同字符串的调用同样会检查注册表，发现存在与该字符串对应的符号，然后就会返回该符号实例。

   ```js
   let fooGlobalSymbol = Symbol.for('foo');       // 创建新符号 
   let otherFooGlobalSymbol = Symbol.for('foo');  // 重用已有符号
   
   console.log(fooGlobalSymbol === otherFooGlobalSymbol);  // true
   ```

   即使采用相同的符号描述，在全局注册表中定义的符号跟使用`Symbol()`定义的符号也并不等同：

   ```js
   let localSymbol = Symbol('foo'); 
   let globalSymbol = Symbol.for('foo');
   
   console.log(localSymbol === globalSymbol); // false
   ```

   全局注册表中的符号必须使用字符串键来创建，因此**作为参数传给`Symbol.for()`的任何值都会被转换为字符串。此外，注册表中使用的键同时也会被用作符号描述。**

   ```js
   let emptyGlobalSymbol = Symbol.for(); 
   console.log(emptyGlobalSymbol);    // Symbol(undefined) 
   ```

   还可以使用`Symbol.keyFor()`来查询全局注册表，这个方法接收符号，返回该全局符号对应的字符串键。如果查询的不是全局符号，则返回`undefined`。

   ```js
   // 创建全局符号 
   let s = Symbol.for('foo'); 
   console.log(Symbol.keyFor(s));   // foo 
   
    
   // 创建普通符号 
   let s2 = Symbol('bar'); 
   console.log(Symbol.keyFor(s2));  // undefined
   ```

   如果传给`Symbol.keyFor()`的不是符号，则该方法抛出`TypeError`：

   ```js
   Symbol.keyFor(123); // TypeError: 123 is not a symbol
   ```

3. 使用符号作为属性

   凡是可以使用字符串或数值作为属性的地方，都可以使用符号。这就包括了对象字面量属性和`Object.defineProperty()/Object.defineProperties()`定义的属性。对象字面量只能在计算属性语法中使用符号作为属性。

   ```js
   let s1 = Symbol('foo'), 
       s2 = Symbol('bar'), 
       s3 = Symbol('baz'), 
   
       s4 = Symbol('qux'); 
    
   let o = { 
   
     [s1]: 'foo val' 
   }; 
   // 这样也可以：o[s1] = 'foo val'; 
    
   console.log(o); 
   // {Symbol(foo): foo val} 
   
    
   Object.defineProperty(o, s2, {value: 'bar val'}); 
    
   
   console.log(o); 
   // {Symbol(foo): foo val, Symbol(bar): bar val} 
    
   
   Object.defineProperties(o, { 
     [s3]: {value: 'baz val'}, 
     [s4]: {value: 'qux val'} 
   
   }); 
    
   console.log(o); 
   
   // {Symbol(foo): foo val, Symbol(bar): bar val, 
   
   //  Symbol(baz): baz val, Symbol(qux): qux val}
   ```

   类似于`Object.getOwnPropertyNames()`返回对象实例的常规属性数组，`Object.getOwnPropertySymbols()`返回对象实例的符号属性数组。这两个方法的返回值彼此互斥。`Object.getOwnPropertyDescriptors()`会返回同时包含常规和符号属性描述符的对象。`Reflect.ownKeys()`会返回两种类型的键：

   ```js
   let s1 = Symbol('foo'), 
       s2 = Symbol('bar');  
   
    
   let o = { 
   
     [s1]: 'foo val', 
     [s2]: 'bar val', 
     baz: 'baz val', 
   
     qux: 'qux val' 
   }; 
    
   
   console.log(Object.getOwnPropertySymbols(o)); 
   // [Symbol(foo), Symbol(bar)] 
    
   
   console.log(Object.getOwnPropertyNames(o)); 
   // ["baz", "qux"] 
    
   
   console.log(Object.getOwnPropertyDescriptors(o)); 
   // {baz: {...}, qux: {...}, Symbol(foo): {...}, Symbol(bar): {...}} 
    
   
   console.log(Reflect.ownKeys(o)); 
   
   // ["baz", "qux", Symbol(foo), Symbol(bar)] 
   ```

   因为符号属性是对内存中符号的一个引用，所以直接创建并用作属性的符号不会丢失。但是，如果没有显式地保存对这些属性的引用，那么必须遍历对象的所有符号属性才能找到相应的属性键： 

   ```js
   let o = {  
   
     [Symbol('foo')]: 'foo val', 
     [Symbol('bar')]: 'bar val' 
   }; 
   
    
   console.log(o); 
   // {Symbol(foo): "foo val", Symbol(bar): "bar val"} 
   
    
   let barSymbol = Object.getOwnPropertySymbols(o) 
                 .find((symbol) => symbol.toString().match(/bar/)); 
   
    
   console.log(barSymbol); 
   // Symbol(bar)
   ```

4. 常用内置符号

   `ECMAScript 6` 也引入了一批常用内置符号（`well-known symbol`），用于暴露语言内部行为，开发者可以直接访问、重写或模拟这些行为。这些内置符号都以`Symbol` 工厂函数字符串属性的形式存在。 

   

   这些内置符号最重要的用途之一是重新定义它们，从而改变原生结构的行为。比如，`for-of` 循环会在相关对象上使用`Symbol.iterator` 属性，那么就可以通过在自定义对象上重新定义`Symbol.iterator` 的值，来改变`for-of` 在迭代该对象时的行为。 这些内置符号也没有什么特别之处，它们就是全局函数`Symbol` 的普通字符串属性，指向一个符号的实例。所有内置符号属性都是不可写、不可枚举、不可配置的。

   - `Symbol.asyncIterator`

     根据`ECMAScript` 规范，这个符号作为一个属性表示“一个方法，该方法返回对象默认的`AsyncIterator`。由`for-await-of` 语句使用”。换句话说，这个符号表示实现异步迭代器`API` 的函数。

     `for-await-of` 循环会利用这个函数执行异步迭代操作。循环时，它们会调用以`Symbol.asyncIterator`为键的函数，并期望这个函数会返回一个实现迭代器`API` 的对象。

     
   
     技术上，这个由 `Symbol.asyncIterator` 函数生成的对象应该通过其 next()方法陆续返回`Promise` 实例。可以通过显式地调用`next()`方法返回，也可以隐式地通过异步生成器函数返回：
   
     ```js
     class Emitter {  
       constructor(max) { 
         this.max = max; 
         this.asyncIdx = 0; 
       } 
     
      
       async *[Symbol.asyncIterator]() { 
         while(this.asyncIdx < this.max) { 
     
           yield new Promise((resolve) => resolve(this.asyncIdx++)); 
         } 
       } 
     
     } 
      
     async function asyncCount() { 
     
       let emitter = new Emitter(5); 
      
       for await(const x of emitter) { 
     
         console.log(x); 
       } 
     } 
     
      
     asyncCount(); 
     // 0 
     
     // 1 
     // 2 
     // 3 
     // 4 
     ```
   
     也可通过以下方式使用：
   
     ```js
     const myAsyncIterable = new Object();
     myAsyncIterable[Symbol.asyncIterator] = async function*() {
         yield "hello";
         yield "async";
         yield "iteration!";
     };
     
     (async () => {
         for await (const x of myAsyncIterable) {
             console.log(x);
             // expected output:
             //    "hello"
             //    "async"
             //    "iteration!"
         }
     })();
     
     ```
   
   - `Symbol.hasInstance`
   
     根据`ECMAScript` 规范，这个符号作为一个属性表示“一个方法，该方法决定一个构造器对象是否认可一个对象是它的实例。由`instanceof` 操作符使用”。`instanceof` 操作符可以用来确定一个对象实例的原型链上是否有原型。`instanceof` 的典型使用场景如下：
   
     ```js
     function Foo() {} 
     let f = new Foo(); 
     console.log(f instanceof Foo); // true 
      
     class Bar {} 
     
     let b = new Bar(); 
     
     console.log(b instanceof Bar); // true
     ```
   
     在 `ES6` 中，`instanceof` 操作符会使用 `Symbol.hasInstance` 函数来确定关系。以 `Symbol.hasInstance` 为键的函数会执行同样的操作，只是操作数对调了一下：
   
     ```js
     function Foo() {} 
     let f = new Foo(); 
     console.log(Foo[Symbol.hasInstance](f)); // true 
     
      
     class Bar {} 
     let b = new Bar(); 
     console.log(Bar[Symbol.hasInstance](b)); // true
     ```
   
     这个属性定义在`Function` 的原型上，因此默认在所有函数和类上都可以调用。由于`instanceof`操作符会在原型链上寻找这个属性定义，就跟在原型链上寻找其他属性一样，因此可以在继承的类上通过静态方法重新定义这个函数：
   
     ```js
     class Bar {}  
     class Baz extends Bar { 
       static [Symbol.hasInstance]() { 
         return false; 
     
       } 
     } 
      
     
     let b = new Baz(); 
     console.log(Bar[Symbol.hasInstance](b)); // true 
     console.log(b instanceof Bar);           // true 
     
     console.log(Baz[Symbol.hasInstance](b)); // false 
     console.log(b instanceof Baz);           // false
     ```
   
   - `Symbol.isConcatSpreadable`
   
     根据`ECMAScript` 规范，这个符号作为一个属性表示“一个布尔值，如果是true，则意味着对象应该用`Array.prototype.concat()`打平其数组元素”。`ES6` 中的`Array.prototype.concat()`方法会根据接收到的对象类型选择如何将一个类数组对象拼接成数组实例。覆盖 `Symbol.isConcatSpreadable `的值可以修改这个行为。
   
     
   
     当变量为数组的时候，默认值为`undefined`，此时是可以展开的，且`true`值也可以展开，如需不要展开，将数组的`Symbol.isConcatSpreadable`值定义为`false`即可。
   
     当变量为对象的时候，默认值为`undefined`，此时是不可以展开的，且`false`也不展开，如需展开，将对象的`Symbol.isConcatSpreadable`值定义为true即可。
   
     ```js
     let initial = ['foo']; 
     
      
     let array = ['bar']; 
     console.log(array[Symbol.isConcatSpreadable]);  // undefined 
     
     console.log(initial.concat(array));             // ['foo', 'bar'] 
     array[Symbol.isConcatSpreadable] = false; 
     console.log(initial.concat(array));             // ['foo', Array(1)] 
     let arrayLikeObject = { length: 1, 0: 'baz' }; 
     console.log(arrayLikeObject[Symbol.isConcatSpreadable]);  // undefined 
     console.log(initial.concat(arrayLikeObject));             // ['foo', {...}] 
     
     arrayLikeObject[Symbol.isConcatSpreadable] = true; 
     console.log(initial.concat(arrayLikeObject));             // ['foo', 'baz'] 
      
     
     let otherObject = new Set().add('qux'); 
     console.log(otherObject[Symbol.isConcatSpreadable]);  // undefined 
     console.log(initial.concat(otherObject));             // ['foo', Set(1)] 
     
     otherObject[Symbol.isConcatSpreadable] = true; 
     
     console.log(initial.concat(otherObject));             // ['foo'] 
     ```
   
   - `Symbol.iterator`
   
     根据`ECMAScript` 规范，这个符号作为一个属性表示“一个方法，该方法返回对象默认的迭代器。由`for-of` 语句使用”。换句话说，这个符号表示实现迭代器`API` 的函数。
   
     技术上，这个由`Symbol.iterator` 函数生成的对象应该通过其`next()`方法陆续返回值。可以通过显式地调用`next()`方法返回，也可以隐式地通过生成器函数返回：
   
     ```js
     class Emitter {  
     
       constructor(max) { 
         this.max = max; 
         this.idx = 0; 
       } 
     
      
       *[Symbol.iterator]() { 
         while(this.idx < this.max) { 
     
           yield this.idx++; 
         } 
       } 
     
     } 
      
     function count() { 
     
       let emitter = new Emitter(5); 
      
       for (const x of emitter) { 
     
         console.log(x); 
       } 
     } 
     
      
     count(); 
     // 0
     // 1 
     // 2 
     // 3 
     // 4
     ```
   
   - `Symbol.match`
   
     根据`ECMAScript` 规范，这个符号作为一个属性表示“一个正则表达式方法，该方法用正则表达式去匹配字符串。由`String.prototype.match()`方法使用”。`String.prototype.match()`方法会使用以 `Symbol.match` 为键的函数来对正则表达式求值。正则表达式的原型上默认有这个函数的定义，因此所有正则表达式实例默认是这个`String` 方法的有效参数：
   
     ```js
     console.log(RegExp.prototype[Symbol.match]); 
     // ƒ [Symbol.match]() { [native code] } 
      
     
     console.log('foobar'.match(/bar/)); 
     // ["bar", index: 3, input: "foobar", groups: undefined]
     ```
   
     给这个方法传入非正则表达式值会导致该值被转换为`RegExp` 对象。如果想改变这种行为，让方法直接使用参数，则可以重新定义 `Symbol.match` 函数以取代默认对正则表达式求值的行为，从而让`match()`方法使用非正则表达式实例。`Symbol.match` 函数接收一个参数，就是调用 `match()`方法的字符串实例。返回的值没有限制：
   
     ```js
     class FooMatcher {  
       static [Symbol.match](target) { 
         return target.includes('foo'); 
     
       } 
     } 
      
     
     console.log('foobar'.match(FooMatcher)); // true 
     console.log('barbaz'.match(FooMatcher)); // false 
      
     
     class StringMatcher { 
       constructor(str) { 
         this.str = str; 
       } 
      
       [Symbol.match](target) { 
         return target.includes(this.str); 
       } 
     } 
     
      
     console.log('foobar'.match(new StringMatcher('foo'))); // true 
     console.log('barbaz'.match(new StringMatcher('qux'))); // false
     ```
   
   - `Symbol.replace`
   
     根据`ECMAScript` 规范，这个符号作为一个属性表示“一个正则表达式方法，该方法替换一个字符串中匹配的子串。由 `String.prototype.replace()`方法使用”。`String.prototype.replace()`方法会使用以 `Symbol.replace` 为键的函数来对正则表达式求值。正则表达式的原型上默认有这个函数的定义，因此所有正则表达式实例默认是这个`String` 方法的有效参数：
   
     ```js
     console.log(RegExp.prototype[Symbol.replace]); 
     // ƒ [Symbol.replace]() { [native code] } 
      
     console.log('foobarbaz'.replace(/bar/, 'qux')); 
     // 'fooquxbaz'
     ```
   
     给这个方法传入非正则表达式值会导致该值被转换为`RegExp` 对象。如果想改变这种行为，让方法直接使用参数，可以重新定义 `Symbol.replace` 函数以取代默认对正则表达式求值的行为，从而让`replace()`方法使用非正则表达式实例。`Symbol.replace` 函数接收两个参数，即调用replace()方法的字符串实例和替换字符串。返回的值没有限制：
   
     ```js
     class FooReplacer { 
       static [Symbol.replace](target, replacement) { 
         return target.split('foo').join(replacement); 
     
       } 
     }  
      
     
     console.log('barfoobaz'.replace(FooReplacer, 'qux')); 
     // "barquxbaz" 
      
     
     class StringReplacer { 
       constructor(str) { 
         this.str = str; 
     
       } 
      
       [Symbol.replace](target, replacement) { 
     
         return target.split(this.str).join(replacement); 
       } 
     } 
     
      
     console.log('barfoobaz'.replace(new StringReplacer('foo'), 'qux')); 
     // "barquxbaz"
     ```
   
   - `Symbol.search`
   
     根据`ECMAScript` 规范，这个符号作为一个属性表示“一个正则表达式方法，该方法返回字符串中匹配正则表达式的索引。由`String.prototype.search()`方法使用”。`String.prototype.search()`方法会使用以`Symbol.search` 为键的函数来对正则表达式求值。正则表达式的原型上默认有这个函数的定义，因此所有正则表达式实例默认是这个`String` 方法的有效参数：
   
     ```js
     console.log(RegExp.prototype[Symbol.search]); 
     // ƒ [Symbol.search]() { [native code] } 
      
     console.log('foobar'.search(/bar/)); 
     
     // 3 
     ```
   
     给这个方法传入非正则表达式值会导致该值被转换为`RegExp` 对象。如果想改变这种行为，让方法直接使用参数，可以重新定义 `Symbol.search` 函数以取代默认对正则表达式求值的行为，从而让`search()`方法使用非正则表达式实例。`Symbol.search` 函数接收一个参数，就是调用 `match()`方法的字符串实例。返回的值没有限制：
   
     ```js
     class FooSearcher {  
       static [Symbol.search](target) { 
         return target.indexOf('foo'); 
       } 
     }
     console.log('foobar'.search(FooSearcher)); // 0 
     
     console.log('barfoo'.search(FooSearcher)); // 3 
     console.log('barbaz'.search(FooSearcher)); // -1 
      
     
     class StringSearcher { 
       constructor(str) { 
         this.str = str; 
       } 
      
       [Symbol.search](target) { 
         return target.indexOf(this.str); 
       } 
     } 
     
      
     console.log('foobar'.search(new StringSearcher('foo'))); // 0 
     console.log('barfoo'.search(new StringSearcher('foo'))); // 3 
     console.log('barbaz'.search(new StringSearcher('qux'))); // -1 
     ```
   
   - `Symbol.species`
   
     根据`ECMAScript` 规范，这个符号作为一个属性表示“一个函数值，该函数作为创建派生对象的构造函数”。这个属性在内置类型中最常用，用于对内置类型实例方法的返回值暴露实例化派生对象的方法。用`Symbol.species` 定义静态的获取器（`getter`）方法，可以覆盖新创建实例的原型定义：
   
     ```js
     class Array1 extends Array {
       static get [Symbol.species]() { return Array; }
     }
     
     const a = new Array1(1, 2, 3);
     const mapped = a.map(x => x * x);
     
     console.log(mapped instanceof Array1);
     // expected output: false
     
     console.log(mapped instanceof Array);
     // expected output: true
     ```
   
   - `Symbol.split`
   
     根据`ECMAScript` 规范，这个符号作为一个属性表示“一个正则表达式方法，该方法在匹配正则表达式的索引位置拆分字符串。由 `String.prototype.split()`方法使用”。`String.prototype.split()`方法会使用以 `Symbol.split` 为键的函数来对正则表达式求值。正则表达式的原型上默认有这个函数的定义，因此所有正则表达式实例默认是这个`String` 方法的有效参数：
   
     ```js
     console.log(RegExp.prototype[Symbol.split]); 
     // ƒ [Symbol.split]() { [native code] } 
      
     
     console.log('foobarbaz'.split(/bar/)); 
     // ['foo', 'baz'] 
     ```
   
     给这个方法传入非正则表达式值会导致该值被转换为`RegExp` 对象。如果想改变这种行为，让方法直接使用参数，可以重新定义`Symbol.split` 函数以取代默认对正则表达式求值的行为，从而让`split()`方法使用非正则表达式实例。`Symbol.split` 函数接收一个参数，就是调用 `match()`方法的字符串实例。返回的值没有限制：
   
     ```js
     class FooSplitter {  
       static [Symbol.split](target) { 
         return target.split('foo'); 
       } 
     
     } 
      
     console.log('barfoobaz'.split(FooSplitter)); 
     
     // ["bar", "baz"] 
      
     class StringSplitter { 
       constructor(str) { 
         this.str = str; 
       } 
     
       [Symbol.split](target) { 
         return target.split(this.str); 
       } 
     } 
      
     
     console.log('barfoobaz'.split(new StringSplitter('foo'))); 
     // ["bar", "baz"]
     ```
   
   - `Symbol.toPrimitive`
   
     根据`ECMAScript` 规范，这个符号作为一个属性表示“一个方法，该方法将对象转换为相应的原始值。由`ToPrimitive` 抽象操作使用”。很多内置操作都会尝试强制将对象转换为原始值，包括字符串、数值和未指定的原始类型。对于一个自定义对象实例，通过在这个实例的`Symbol.toPrimitive` 属性上定义一个函数可以改变默认行为。
   
     根据提供给这个函数的参数（`string`、`number` 或`default`），可以控制返回的原始值：
   
     ```js
     class Foo {}  
     let foo = new Foo(); 
     
     console.log(3 + foo);       // "3[object Object]" 
     console.log(3 - foo);       // NaN 
     console.log(String(foo));   // "[object Object]" 
     
      
     
     class Bar { 
       constructor() { 
         this[Symbol.toPrimitive] = function(hint) { 
           switch (hint) { 
             case 'number': 
               return 3; 
             case 'string': 
               return 'string bar'; 
             case 'default': 
             default: 
               return 'default bar'; 
           } 
         } 
       } 
     }
     
      
     let bar = new Bar();
     console.log(3 + bar);     // "3default bar" 
     console.log(3 - bar);     // 0
     console.log(String(bar)); // string bar
     ```
   
   - `Symbol.toStringTag`
   
     根据`ECMAScript` 规范，这个符号作为一个属性表示“一个字符串，该字符串用于创建对象的默认字符串描述。由内置方法`Object.prototype.toString()`使用”。
   
     通过`toString()`方法获取对象标识时，会检索由`Symbol.toStringTag` 指定的实例标识符，默认为"Object"。内置类型已经指定了这个值，但自定义类实例还需要明确定义:
   
     ```js
     let s = new Set(); 
     
     console.log(s);                      // Set(0) {} 
     console.log(s.toString());           // [object Set] 
     console.log(s[Symbol.toStringTag]);  // Set 
     
     class Foo {}  
     
     let foo = new Foo(); 
     
     console.log(foo);                      // Foo {} 
     console.log(foo.toString());           // [object Object] 
     console.log(foo[Symbol.toStringTag]);  // undefined 
     
     class Bar { 
       constructor() { 
         this[Symbol.toStringTag] = 'Bar'; 
       } 
     } 
     
     let bar = new Bar(); 
     
     console.log(bar);                      // Bar {} 
     
     console.log(bar.toString());           // [object Bar] 
     console.log(bar[Symbol.toStringTag]);  // Bar
     ```
   
   - `Symbol.unscopables`
   
     根据`ECMAScript` 规范，这个符号作为一个属性表示“一个对象，该对象所有的以及继承的属性，都会从关联对象的`with` 环境绑定中排除”。设置这个符号并让其映射对应属性的键值为true，就可以阻止该属性出现在with 环境绑定中，如下例所示:
   
     ```js
     let o = { foo: 'bar' }; 
     
     with (o) {  
       console.log(foo); // bar 
     } 
     
     o[Symbol.unscopables] = { 
       foo: true 
     }; 
     
     with (o) { 
       console.log(foo);  //ReferenceError 
     }
     ```
   
     
   
     不推荐使用`with`，因此也不推荐使用`Symbol.unscopables`。
   

## Object类型

`ECMAScript` 中的对象其实就是一组数据和功能的集合。对象通过`new` 操作符后跟对象类型的名称来创建。开发者可以通过创建`Object` 类型的实例来创建自己的对象，然后再给对象添加属性和方法：

```js
let o = new Object();
```

`Object` 的实例本身并不是很有用，但理解与它相关的概念非常重要。类似`Java` 中的`java.lang.Object`，`ECMAScript` 中的`Object` 也是派生其他对象的基类。`Object` 类型的所有属性和方法在派生的对象上同样存在。

每个`Object` 实例都有如下属性和方法。

1. `constructor`

   用于创建当前对象的函数。在前面的例子中，这个属性的值就是 `Object()` 函数。

2. `hasOwnProperty(propertyName)`

   用于判断当前对象实例（不是原型）上是否存在给定的属性。要检查的属性名必须是字符串（如`o.hasOwnProperty("name")`）或符号。

3. `isPrototypeOf(object)`

   用于判断当前对象是否为另一个对象的原型。

4.  `propertyIsEnumerable(propertyName)`

   用于判断给定的属性是否可以使用`for-in` 语句枚举。与`hasOwnProperty()`一样，属性名必须是字符串。

5. `toLocaleString()`

   返回对象的字符串表示，该字符串反映对象所在的本地化执行环境。

6. `toString()`

   返回对象的字符串表示

7. `valueOf()`

   返回对象对应的字符串、数值或布尔值表示。通常与`toString()`的返回值相同



因为在`ECMAScript` 中`Object` 是所有对象的基类，所以任何对象都有这些属性和方法。

# 操作符

`ECMA-262` 描述了一组可用于操作数据值的操作符，包括数学操作符（如加、减）、位操作符、关系操作符和相等操作符等。`ECMAScript` 中的操作符是独特的，因为它们可用于各种值，包括字符串、数值、布尔值，甚至还有对象。在应用给对象时，操作符通常会调用 `valueOf()`和/或 `toString()`方法来取得可以计算的值。



1. 一元操作符

   只操作一个值的操作符叫一元操作符（`unary operator`）。

   - 递增/递减操作符

     ```js
     let age = 29; 
     
     let anotherAge = --age + 2; 
      
     console.log(age);         // 28 
     console.log(anotherAge);  // 30
     ```

     在这个例子中，变量 `anotherAge` 以 `age` 减 1 后的值再加2 进行初始化。因为递减操作先发生，所以`age` 的值先变成28，然后再加2，结果是30。

     前缀递增和递减在语句中的优先级是相等的，因此会从左到右依次求值。比如：

     ```js
     let num1 = 2; 
     let num2 = 20; 
     let num3 = --num1 + num2; 
     let num4 = num1 + num2; 
     
     console.log(num3);  // 21 
     
     console.log(num4);  // 21
     ```

     这里，`num3` 等于21 是因为`num1` 先减1 之后才加`num2`。变量`num4` 也是21，那是因为加法使用的也是递减后的值。

     递增和递减的后缀版语法一样（分别是++和--），只不过要放在变量后面。后缀版与前缀版的主要区别在于，后缀版递增和递减在语句被求值后才发生。在某些情况下，这种差异没什么影响，可是，在跟其他操作混合时，差异就会变明显，比如：

     ```js
     let num1 = 2; 
     let num2 = 20; 
     let num3 = num1-- + num2; 
     let num4 = num1 + num2;
     console.log(num3);  // 22 
     console.log(num4);  // 21
     ```

     

   - 一元加和减

     一元加和减操作符对大多数开发者来说并不陌生，它们在`ECMAScript` 中跟在高中数学中的用途一样。一元加由一个加号（+）表示，放在变量前头，对数值没有任何影响：

     ```js
     let num = 25; 
     num = +num; 
     console.log(num); // 25
     ```

     如果将一元加应用到非数值，则会执行与使用`Number()`转型函数一样的类型转换：布尔值false和true 转换为0 和1，字符串根据特殊规则进行解析，对象会调用它们的`valueOf()`和/或`toString()`方法以得到可以转换的值。

2. 布尔操作符

   对于编程语言来说，布尔操作符跟相等操作符几乎同样重要。如果没有能力测试两个值的关系，那么像`if-else` 和循环这样的语句也没什么用了。布尔操作符一共有3 个：逻辑非、逻辑与和逻辑或。

   - 逻辑非

     逻辑非操作符由一个叹号（!）表示，可应用给 `ECMAScript` 中的任何值。这个操作符始终返回布尔值，无论应用到的是什么数据类型。逻辑非操作符首先将操作数转换为布尔值，然后再对其取反。换句话说，逻辑非操作符会遵循如下规则。

     1. 如果操作数是对象，则返回false。
     2. 如果操作数是空字符串，则返回true。
     3. 如果操作数是非空字符串，则返回false。
     4. 如果操作数是数值0，则返回true。
     5. 如果操作数是非0 数值（包括Infinity），则返回false。
     6. 如果操作数是null，则返回true。
     7. 如果操作数是NaN，则返回true。
     8. 如果操作数是undefined，则返回true。 

     以下示例验证了上述行为：

     ```js
     console.log(!false);   // true 
     console.log(!"blue");  // false 
     console.log(!0);       // true 
     console.log(!NaN);     // true 
     console.log(!"");      // true 
     console.log(!12345);   // false
     ```

     逻辑非操作符也可以用于把任意值转换为布尔值。同时使用两个叹号（!!），相当于调用了转型函数 `Boolean()`。无论操作数是什么类型，第一个叹号总会返回布尔值。第二个叹号对该布尔值取反，从而给出变量真正对应的布尔值。结果与对同一个值使用`Boolean()`函数是一样的。

   - 逻辑与

     逻辑与操作符由两个和号（&&）表示，应用到两个值，如下所示：

     ```js
     let result = true && false;
     ```

     逻辑与操作符是一种短路操作符，意思就是如果第一个操作数决定了结果，那么永远不会对第二个操作数求值。对逻辑与操作符来说，如果第一个操作数是false，那么无论第二个操作数是什么值，结果也不可能等于true。

   - 逻辑或

     逻辑或操作符由两个管道符（||）表示，比如：

     ```js
     let result = true || false;
     ```

     与逻辑与类似，逻辑或操作符也具有短路的特性。只不过对逻辑或而言，第一个操作数求值为true，第二个操作数就不会再被求值了。

3. 乘性操作符

   `ECMAScript` 定义了3 个乘性操作符：乘法、除法和取模。这些操作符跟它们在Java、C 语言及Perl中对应的操作符作用一样，但在处理非数值时，它们也会包含一些自动的类型转换。如果乘性操作符有不是数值的操作数，则该操作数会在后台被使用`Number()`转型函数转换为数值。这意味着空字符串会被当成0，而布尔值true 会被当成1。

4. 指数操作符

   `ECMAScript` 7 新增了指数操作符，`Math.pow()`现在有了自己的操作符`**`，结果是一样的：

   ```js
   console.log(Math.pow(3, 2);    // 9 
   console.log(3 ** 2);           // 9 
    
   
   console.log(Math.pow(16, 0.5); // 4 
   
   console.log(16** 0.5);         // 4
   ```

   

5. 加性操作符

   加性操作符，即加法和减法操作符，一般都是编程语言中最简单的操作符。不过，在 `ECMAScript`中，这两个操作符拥有一些特殊的行为。与乘性操作符类似，加性操作符在后台会发生不同数据类型的转换。

6. 关系操作符

   关系操作符执行比较两个值的操作，包括小于（<）、大于（>）、小于等于（<=）和大于等于（>=），用法跟数学课上学的一样。这几个操作符都返回布尔值。

   与`ECMAScript` 中的其他操作符一样，在将它们应用到不同数据类型时也会发生类型转换和其他行为。

   1. 如果操作数都是数值，则执行数值比较。 

   2. 如果操作数都是字符串，则逐个比较字符串中对应字符的编码。 因为比较的是字符对应的编码的大小，所以会发生：

      ```js
      let result = "Brick" < "alphabet"; // true 
      ```

   3. 如果有任一操作数是数值，则将另一个操作数转换为数值，执行数值比较。 

   4. 如果有任一操作数是对象，则调用其`valueOf()`方法，取得结果后再根据前面的规则执行比较。如果没有`valueOf()`操作符，则调用`toString()`方法，取得结果后再根据前面的规则执行比较。 

   5. 如果有任一操作数是布尔值，则将其转换为数值再执行比较。

7. 相等操作符

   判断两个变量是否相等是编程中最重要的操作之一。在比较字符串、数值和布尔值是否相等时，过程都很直观。但是在比较两个对象是否相等时，情形就比较复杂了。`ECMAScript` 中的相等和不相等操作符，原本在比较之前会执行类型转换，但很快就有人质疑这种转换是否应该发生。最终，`ECMAScript`提供了两组操作符。第一组是等于和不等于，它们在比较之前执行转换。第二组是全等和不全等，它们在比较之前不执行转换。

   1. 等于和不等于

      `ECMAScript` 中的等于操作符用两个等于号（==）表示，如果操作数相等，则会返回`true`。不等于操作符用叹号和等于号（!=）表示，如果两个操作数不相等，则会返回 true。这两个操作符都会先进行类型转换（通常称为强制类型转换）再确定操作数是否相等。

      在转换操作数的类型时，相等和不相等操作符遵循如下规则。

      - 如果任一操作数是布尔值，则将其转换为数值再比较是否相等。false 转换为0，true 转换为1。 
      - 如果一个操作数是字符串，另一个操作数是数值，则尝试将字符串转换为数值，再比较是否相等。 
      - 如果一个操作数是对象，另一个操作数不是，则调用对象的 `valueOf()`方法取得其原始值，再根据前面的规则进行比较。 

      

      在进行比较时，这两个操作符会遵循如下规则。

      - `null` 和`undefined` 相等。 
      - `null` 和`undefined` 不能转换为其他类型的值再进行比较。 
      - 如果有任一操作数是`NaN`，则相等操作符返回`false`，不相等操作符返回`true`。记住：即使两个操作数都是`NaN`，相等操作符也返回`false`，因为按照规则，`NaN` 不等于`NaN`。 
      - 如果两个操作数都是对象，则比较它们是不是同一个对象。如果两个操作数都指向同一个对象，则相等操作符返回true。否则，两者不相等。

      

      下表总结了一些特殊情况及比较的结果。

      | 表达式            | 结果  |
      | ----------------- | ----- |
      | null == undefined | true  |
      | "NaN" == NaN      | false |
      | 5 == NaN          | false |
      | NaN == NaN        | false |
      | NaN != NaN        | true  |
      | false == 0        | true  |
      | true == 1         | true  |
      | true == 2         | false |
      | undefined == 0    | false |
      | null == 0         | false |
      | "5" == 5          | true  |

   2. 全等和不全等

      全等和不全等操作符与相等和不相等操作符类似，只不过它们在比较相等时不转换操作数。全等操作符由3 个等于号（===）表示，只有两个操作数在不转换的前提下相等才返回`true`，比如：

      ```js
      let result1 = ("55" == 55);   // true，转换后相等 
      let result2 = ("55" === 55);  // false，不相等，因为数据类型不同
      ```

      不全等操作符用一个叹号和两个等于号（!==）表示，**只有两个操作数在不转换的前提下不相等才返回true**。比如：

      ```js
      let result1 = ("55" != 55);  // false，转换后相等 
      let result2 = ("55" !== 55); // true，不相等，因为数据类型不同 
      ```

      另外，虽然 `null == undefined` 是 `true`（因为这两个值类似），但 `null === undefined` 是`false`，因为它们不是相同的数据类型。

8. 条件操作符

   条件操作符是`ECMAScript` 中用途最为广泛的操作符之一，语法跟Java 中一样：

   ```js
   variable = boolean_expression ? true_value : false_value;
   ```

9. 赋值操作符

   简单赋值用等于号（=）表示，将右手边的值赋给左手边的变量，如下所示：

   ```js
   let num = 10;
   ```

   复合赋值使用乘性、加性或位操作符后跟等于号（=）表示。这些赋值操作符是类似如下常见赋值操作的简写形式：

   ```js
   let num = 10; 
   num = num + 10;
   ```

   以上代码的第二行可以通过复合赋值来完成：

   ```json
   let num = 10; 
   num += 10;
   ```

   每个数学操作符以及其他一些操作符都有对应的复合赋值操作符：

   1. 乘后赋值（*=） 
   2. 除后赋值（/=） 
   3. 取模后赋值（%=） 
   4. 加后赋值（+=） 
   5. 减后赋值（-=） 
   6. 左移后赋值（<<=） 
   7. 右移后赋值（>>=） 
   8. 无符号右移后赋值（>>>=）

   

    这些操作符仅仅是简写语法，使用它们不会提升性能。

10. 逗号操作符

   逗号操作符可以用来在一条语句中执行多个操作，如下所示：

   ```js
   let num1 = 1, num2 = 2, num3 = 3;
   ```

   

# 语句

`ECMA-262` 描述了一些语句（也称为流控制语句），而`ECMAScript`中的大部分语法都体现在语句中。语句通常使用一或多个关键字完成既定的任务。语句可以简单，也可以复杂。简单的如告诉函数退出，复杂的如列出一堆要重复执行的指令。



1. `if`语句

   `if `语句是使用最频繁的语句之一，语法如下：

   ```js
   if (condition) statement1 else statement2
   ```

   这里的条件（`condition`）可以是任何表达式，并且求值结果不一定是布尔值。`ECMAScript` 会自动调用 `Boolean()`函数将这个表达式的值转换为布尔值。

2. `do-while`语句

   `do-while` 语句是一种后测试循环语句，即循环体中的代码执行后才会对退出条件进行求值。换句话说，循环体内的代码至少执行一次。`do-while` 的语法如下：

   ```js
   do { 
     statement  
   } while (expression); 
   ```

   后测试循环经常用于这种情形：循环体内代码在退出前至少要执行一次。

3. `while`语句

   `while` 语句是一种先测试循环语句，即先检测退出条件，再执行循环体内的代码。因此，`while` 循环体内的代码有可能不会执行。下面是`while` 循环的语法：

   ```js
   while(expression) statement
   ```

4. `for` 语句

   `for` 语句也是先测试语句，只不过增加了进入循环之前的初始化代码，以及循环执行后要执行的表达式，语法如下：

   ```js
   for (initialization; expression; post-loop-expression) statement
   ```

   在for 循环的初始化代码中，其实是可以不使用变量声明关键字的。不过，初始化定义的迭代器变量在循环执行完成后几乎不可能再用到了。因此，最清晰的写法是使用`let` 声明迭代器变量，这样就可以将这个变量的作用域限定在循环中。

   初始化、条件表达式和循环后表达式都不是必需的。因此，下面这种写法可以创建一个无穷循环：

   ```js
   for (;;) { // 无穷循环 
     doSomething();  
   }
   ```

   如果只包含条件表达式，那么for 循环实际上就变成了while 循环：

   ```json
   let count = 10; 
   let i = 0; 
   for (; i < count; ) { 
     console.log(i); 
     i++; 
   }
   ```

5. `for-in` 语句

   `for-in` 语句是一种严格的迭代语句，用于枚举对象中的非符号键属性，语法如下：

   ```js
   for (property in expression) statement
   ```

   下面是一个例子：

   ```js
   for (const propName in window) { 
     document.write(propName);  
   }
   ```

   这个例子使用 `for-in` 循环显示了 `BOM` 对象 `window` 的所有属性。每次执行循环，都会给变量`propName` 赋予一个`window` 对象的属性作为值，直到window 的所有属性都被枚举一遍。与`for` 循环一样，这里控制语句中的`const` 也不是必需的。但为了确保这个局部变量不被修改，推荐使用`const`。

   `ECMAScript` 中对象的属性是无序的，因此`for-in` 语句不能保证返回对象属性的顺序。换句话说，所有可枚举的属性都会返回一次，但返回的顺序可能会因浏览器而异。

   如果`for-in` 循环要迭代的变量是`null` 或`undefined`，则不执行循环体。

6. `for-of` 语句

   for-of 语句是一种严格的迭代语句，用于遍历可迭代对象的元素，语法如下：

   ```js
   for (property of expression) statement 
   ```

   下面是示例：

   ```js
   for (const el of [2,4,6,8]) { 
     document.write(el); 
   }
   ```

   在这个例子中，使用`for-of` 语句显示了一个包含4 个元素的数组中的所有元素。循环会一直持续到将所有元素都迭代完。与`for` 循环一样，这里控制语句中的`const` 也不是必需的。但为了确保这个局部变量不被修改，推荐使用`const`。

   `for-of` 循环会按照**可迭代对象**的`next()`方法产生值的顺序迭代元素。如果尝试迭代的变量不支持迭代，则`for-of` 语句会抛出错误。

7. 标签语句

   标签语句用于给语句加标签，语法如下：

   ```js
   label: statement
   ```

   下面是一个例子：

   ```js
   start: for (let i = 0; i < count; i++) { 
     console.log(i);  
   } 
   ```

   在这个例子中，`start` 是一个标签，可以在后面通过`break` 或`continue` 语句引用。标签语句的典型应用场景是嵌套循环。

8. `break` 和`continue` 语句

   `break` 和`continue` 语句为执行循环代码提供了更严格的控制手段。其中，`break` 语句用于立即退出循环，强制执行循环后的下一条语句。而`continue` 语句也用于立即退出循环，但会再次从循环顶部开始执行。下面看一个例子：

   ```js
   let num = 0;  
    
   for (let i = 1; i < 10; i++) { 
   
     if (i % 5 == 0) { 
       break;
     } 
     num++; 
   } 
    
   console.log(num); // 4
   ```

   如果将`break` 换成`continue`，则会出现不同的效果：

   ```js
   let num = 0;  
    
   for (let i = 1; i < 10; i++) { 
     if (i % 5 == 0) { 
       continue; 
   
     } 
     num++; 
   } 
   
    
   
   console.log(num); // 8
   ```

   `break` 和`continue` 都可以与标签语句一起使用，返回代码中特定的位置。这通常是在嵌套循环中，如下面的例子所示：

   ```js
   let num = 0; 
   
    
   outermost: 
   for (let i = 0; i < 10; i++) { 
   
     for (let j = 0; j < 10; j++) { 
       if (i == 5 && j == 5) { 
         break outermost; 
   
       } 
       num++;  
     } 
   
   } 
    
   console.log(num); // 55 
   ```

   在这个例子中，`outermost` 标签标识的是第一个`for` 语句。正常情况下，每个循环执行10 次，意味着`num++`语句会执行100 次，而循环结束时`console.log` 的结果应该是100。但是，`break`语句带来了一个变数，即要退出到的标签。添加标签不仅让break 退出（使用变量j 的）内部循环，也会退出（使用变量i 的）外部循环。当执行到i 和j 都等于5 时，循环停止执行，此时`num` 的值是55。

   `continue`语句也可以使用标签，如下面的例子所示：

   ```js
   let num = 0; 
    
   outermost:  
   for (let i = 0; i < 10; i++) { 
     for (let j = 0; j < 10; j++) {
     if (i == 5 && j == 5) { 
         continue outermost; 
       } 
       num++; 
   
     } 
   } 
    
   console.log(num); // 95
   ```

   这一次，`continue` 语句会强制循环继续执行，但不是继续执行内部循环，而是继续执行外部循环。当i 和j 都等于5 时，会执行`continue`，跳到外部循环继续执行，从而导致内部循环少执行5 次，结果`num` 等于95。

   组合使用标签语句和`break`、`continue` 能实现复杂的逻辑，但也容易出错。注意标签要使用描述性强的文本，而嵌套也不要太深。

9. `with`语句

   `with` 语句的用途是将代码作用域设置为特定的对象，其语法是：

   ```js
   with (expression) statement;
   ```

   使用`with` 语句的主要场景是针对一个对象反复操作，这时候将代码作用域设置为该对象能提供便利，如下面的例子所示：

   ```js
   let qs = location.search.substring(1); 
   let hostName = location.hostname; 
   let url = location.href;
   ```

   上面代码中的每一行都用到了`location` 对象。如果使用`with` 语句，就可以少写一些代码：

   ```js
   with(location) {  
     let qs = search.substring(1); 
     let hostName = hostname; 
   
     let url = href; 
   
   }
   ```

   这里，`with` 语句用于连接`location` 对象。这意味着在这个语句内部，每个变量首先会被认为是一个局部变量。如果没有找到该局部变量，则会搜索`location`对象，看它是否有一个同名的属性。如果有，则该变量会被求值为`location` 对象的属性。 

   

   严格模式不允许使用`with` 语句，否则会抛出错误。由于`with` 语句影响性能且难于调试其中的代码，通常不推荐在产品代码中使用`with`
   语句。

10. `switch` 语句

    `switch` 语句是与if 语句紧密相关的一种流控制语句，从其他语言借鉴而来。`ECMAScript` 中switch语句跟C 语言中`switch` 语句的语法非常相似，如下所示：
    
    ```json
    switch (expression) { 
      case value1:  
        statement
        break; 
      case value2: 
        statement 
        break; 
    
      case value3: 
        statement 
        break; 
    
      case value4: 
        statement 
        break; 
    
      default: 
        statement 
    } 
    ```
    
    `break`关键字会导致代码执行跳出switch 语句。如果没有break，则代码会继续匹配下一个条件。`default`关键字用于在任何条件都没有满足时指定默认执行的语句（相当于else 语句）。
    
    
    
    虽然`switch` 语句是从其他语言借鉴过来的，但`ECMAScript` 为它赋予了一些独有的特性。首先，`switch` 语句可以用于所有数据类型（在很多语言中，它只能用于数值），因此可以使用字符串甚至对象。其次，条件的值不需要是常量，也可以是变量或表达式。看下面的例子：
    
    ```js
    switch ("hello world") { 
      case "hello" + " world": 
        console.log("Greeting was found."); 
        break; 
    
      case "goodbye": 
        console.log("Closing was found."); 
        break; 
    
      default: 
        console.log("Unexpected message was found."); 
    }
    ```
    
    `switch` 语句在比较每个条件的值时会使用**全等操作符**，因此不会强制转换数据类型（比如，字符串"10"不等于数值10）。



# 函数

函数对任何语言来说都是核心组件，因为它们可以封装语句，然后在任何地方、任何时间执行。`ECMAScript` 中的函数使用`function` 关键字声明，后跟一组参数，然后是函数体。

函数的基本语法：

```js
function functionName(arg0, arg1,...,argN) { 
  statements  
} 
```

`ECMAScript`中的函数不需要指定是否返回值。要注意的是，只要碰到`return` 语句，函数就会立即停止执行并退出。因此，`return` 语句后面的
代码不会被执行。`return` 语句也可以不带返回值。这时候，函数会立即停止执行并返回`undefined`。这种用法最常用于提前终止函数执行，并不是为了返回值。



**最佳实践是函数要么返回值，要么不返回值。只在某个条件下返回值的函数会带来麻烦，尤其是调试时。**



