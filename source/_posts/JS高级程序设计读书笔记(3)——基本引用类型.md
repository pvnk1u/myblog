---
title: "JS高级程序设计读书笔记(3)——基本引用类型" 
date: 2022-08-18T12:32:39+08:00
draft: false
tags: ["web","javascript"]
categories:
  - "web"
  - "javascript"
toc: true
---

**引用值（或者对象）是某个特定引用类型的实例**。在`ECMAScript`中，**引用类型是把数据和功能组织到一起的结构**，经常被人错误地称作“类”。虽然从技术上讲 `JavaScript` 是一门面向对象语言，但`ECMAScript` 缺少传统的面向对象编程语言所具备的某些基本结构，包括类和接口。引用类型有时候也被称为对象定义，因为它们描述了自己的对象应有的属性和方法。





引用类型虽然有点像类，但跟类并不是一个概念。



对象被认为是某个特定引用类型的实例。新对象通过使用`new` 操作符后跟一个构造函数（`constructor`）来创建。构造函数就是用来创建新对象的函数，比如下面这行代码：

```js
let now = new Date();
```

这行代码创建了引用类型 `Date` 的一个新实例，并将它保存在变量 `now` 中。`Date()`在这里就是构造函数，它负责创建一个只有默认属性和方法的简单对象。`ECMAScript` 提供了很多像 `Date` 这样的原生引用类型，帮助开发者实现常见的任务。



函数也是一种引用类型。



# Date

`ECMAScript` 的`Date` 类型参考了`Java` 早期版本中的`java.util.Date`。为此，`Date` 类型将日期保存为自协调世界时（`UTC`，`Universal  Time  Coordinated`）时间1970 年1 月1 日午夜（零时）至今所经过的毫秒数。使用这种存储格式，`Date` 类型可以精确表示1970 年1 月1 日之前及之后285 616 年的日期。



要创建日期对象，就使用`new` 操作符来调用`Date` 构造函数：

在不给`Date` 构造函数传参数的情况下，创建的对象将保存当前日期和时间。要基于其他日期和时间创建日期对象，必须传入其毫秒表示（`UNIX` 纪元1970 年1 月1 日午夜之后的毫秒数）。`ECMAScript`为此提供了两个辅助方法：`Date.parse()`和`Date.UTC()`。



`Date.parse()`方法接收一个表示日期的字符串参数，尝试将这个字符串转换为表示该日期的毫秒数。`ECMA-262` 第5 版定义了`Date.parse()`应该支持的日期格式，填充了第3 版遗留的空白。所有实现都必须支持下列日期格式：

- “月/日/年”，如"5/23/2019"；
- “月名 日, 年”，如"May 23, 2019"；
- “周几 月名 日 年 时:分:秒 时区”，如"Tue May 23 2019 00:00:00 GMT-0700"； 
- ISO  8601 扩展格式“YYYY-MM-DDTHH:mm:ss.sssZ”，如 2019-05-23T00:00:00（只适用于兼容ES5 的实现）。

比如，要创建一个表示“2019 年5 月23 日”的日期对象，可以使用以下代码：

```js
let someDate = new Date(Date.parse("May 23, 2019"));
```

如果传给`Date.parse()`的字符串并不表示日期，则该方法会返回`NaN`。如果直接把表示日期的字符串传给`Date` 构造函数，那么`Date` 会在后台调用`Date.parse()`。换句话说，下面这行代码跟前面那行代码是等价的：

```js
let someDate = new Date("May 23, 2019");
```

这两行代码得到的日期对象相同。



`Date.UTC()`方法也返回日期的毫秒表示，但使用的是跟`Date.parse()`不同的信息来生成这个值。传给`Date.UTC()`的参数是年、零起点月数（1 月是0，2 月是1，以此类推）、日（1~31）、时（0~23）、分、秒和毫秒。这些参数中，只有前两个（年和月）是必需的。如果不提供日，那么默认为1 日。其他参数的默认值都是0。下面是使用`Date.UTC()`的两个例子：

```js
// GMT 时间2000 年1 月1 日零点 
let y2k = new Date(Date.UTC(2000, 0)); 
 
// GMT 时间2005 年5 月5 日下午5 点55 分55 秒 
let allFives = new Date(Date.UTC(2005, 4, 5, 17, 55, 55));
```



这个例子创建了两个日期 。第一个日期是2000 年1 月1 日零点（GMT），2000 代表年，0 代表月（1 月）。因为没有其他参数（日取1，其他取 0），所以结果就是该月第1 天零点。第二个日期表示2005年5 月5 日下午5 点55 分55 秒（GMT）。虽然日期里面涉及的都是5，但月数必须用4，因为月数是零起点的。小时也必须是17，因为这里采用的是24 小时制，即取值范围是0~23。其他参数就都很直观了。



与 `Date.parse()`一样，`Date.UTC()`也会被 Date 构造函数隐式调用，但有一个区别：这种情况下创建的是本地日期，不是 `GMT` 日期。不过 `Date` 构造函数跟 `Date.UTC()`接收的参数是一样的。因此，如果第一个参数是数值，则构造函数假设它是日期中的年，第二个参数就是月，以此类推。前面的例子也可以这样来写：

```json
// 本地时间2000 年1 月1 日零点 
let y2k = new Date(2000, 0); 
 

// 本地时间2005 年5 月5 日下午5 点55 分55 秒 
let allFives = new Date(2005, 4, 5, 17, 55, 55);
```

以上代码创建了与前面例子中相同的两个日期，但这次的两个日期是（由于系统设置决定的）本地时区的日期。



`ECMAScript` 还提供了 `Date.now()`方法，返回表示方法执行时日期和时间的毫秒数。这个方法可以方便地用在代码分析中：

```js
// 起始时间 
let start = Date.now(); 

// 调用函数 
doSomething(); 
 
// 结束时间 
let stop = Date.now(), 
result = stop - start;
```



## 继承的方法

与其他类型一样，`Date` 类型重写了`toLocaleString()`、`toString()`和`valueOf()`方法。但与其他类型不同，重写后这些方法的返回值不一样。`Date` 类型的`toLocaleString()`方法返回与浏览器运行的本地环境一致的日期和时间。这通常意味着格式中包含针对时间的AM（上午）或PM（下午），但不包含时区信息（具体格式可能因浏览器而不同）。`toString()`方法通常返回带时区信息的日期和时间，而时间也是以24 小时制（0~23）表示的。下面给出了`toLocaleString()`和`toString()`返回的2019 年2 月1 日零点的示例（地区为"en-US"的PST，即Pacific Standard Time，太平洋标准时间）：

```js
toLocaleString() - 2/1/2019 12:00:00 AM 

toString() - Thu Feb 1 2019 00:00:00 GMT-0800 (Pacific Standard Time) 
```



`Date` 类型的`valueOf()`方法根本就不返回字符串，这个方法被重写后返回的是日期的毫秒表示。因此，操作符（如小于号和大于号）可以直接使用它返回的值。比如下面的例子：

```js
let date1 = new Date(2019, 0, 1);    // 2019 年1 月1 日 
let date2 = new Date(2019, 1, 1);    // 2019 年2 月1 日 
 

console.log(date1 < date2); // true 
console.log(date1 > date2); // false
```

日期2019 年1 月1 日在2019 年2 月1 日之前，所以说前者小于后者没问题。因为2019 年1 月1 日的毫秒表示小于2019 年2 月1 日的毫秒表示，所以用小于号比较这两个日期时会返回true。这也是确保日期先后的一个简单方式。



## 日期格式化方法

`Date` 类型有几个专门用于格式化日期的方法，它们都会返回字符串：

- `toDateString()`显示日期中的周几、月、日、年（格式特定于实现）； 
- `toTimeString()`显示日期中的时、分、秒和时区（格式特定于实现）； 
- `toLocaleDateString()`显示日期中的周几、月、日、年（格式特定于实现和地区）； 
- `toLocaleTimeString()`显示日期中的时、分、秒（格式特定于实现和地区）； 
- `toUTCString()`显示完整的`UTC` 日期（格式特定于实现）。

```js
var a = new Date();
undefined

a.toDateString()
'Mon Aug 22 2022'

a.toTimeString()
'12:37:11 GMT+0800 (中国标准时间)'

a.toLocaleDateString()
'2022/8/22'

a.toLocaleTimeString()
'12:37:11'

a.toUTCString()
'Mon, 22 Aug 2022 04:37:11 GMT'
```



这些方法的输出与 `toLocaleString()`和 `toString()`一样，会因浏览器而异。因此不能用于在用户界面上一致地显示日期。



## 日期/时间组件方法

`Date` 类型剩下的方法（见下表）直接涉及取得或设置日期值的特定部分。注意表中“UTC 日期”，指的是没有时区偏移（将日期转换为`GMT`）时的日期。

| 方法                             | 说明                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| getTime()                        | 返回日期的毫秒表示；与valueOf()相同                          |
| setTime(milliseconds)            | 设置日期的毫秒表示，从而修改整个日期                         |
| getFullYear()                    | 返回4 位数年（即2019 而不是19）                              |
| getUTCFullYear()                 | 返回UTC 日期的4 位数年                                       |
| setFullYear(year)                | 设置日期的年（year 必须是4 位数）                            |
| setUTCFullYear(year)             | 设置UTC 日期的年（year 必须是4 位数）                        |
| getMonth()                       | 返回日期的月（0 表示1 月，11 表示12 月）                     |
| getUTCMonth()                    | 返回UTC 日期的月（0 表示1 月，11 表示12 月）                 |
| setMonth(month)                  | 设置日期的月（month 为大于0 的数值，大于11 加年）            |
| setUTCMonth(month)               | 设置UTC 日期的月（month 为大于0 的数值，大于11 加年）        |
| getDate()                        | 返回日期中的日（1~31）                                       |
| getUTCDate()                     | 返回UTC 日期中的日（1~31）                                   |
| setDate(date)                    | 设置日期中的日（如果date 大于该月天数，则加月）              |
| setUTCDate(date)                 | 设置UTC 日期中的日（如果date 大于该月天数，则加月）          |
| getDay()                         | 返回日期中表示周几的数值（0 表示周日，6 表示周六）           |
| getUTCDay()                      | 返回UTC 日期中表示周几的数值（0 表示周日，6 表示周六）       |
| getHours()                       | 返回日期中的时（0~23）                                       |
| getUTCHours()                    | 返回UTC 日期中的时（0~23）                                   |
| setHours(hours)                  | 设置日期中的时（如果hours 大于23，则加日）                   |
| setUTCHours(hours)               | 设置UTC 日期中的时（如果hours 大于23，则加日）               |
| getMinutes()                     | 返回日期中的分（0~59）                                       |
| getUTCMinutes()                  | 返回UTC 日期中的分（0~59）                                   |
| setMinutes(minutes)              | 设置日期中的分（如果minutes 大于59，则加时）                 |
| setUTCMinutes(minutes)           | 设置UTC 日期中的分（如果minutes 大于59，则加时）             |
| getSeconds()                     | 返回日期中的秒（0~59）                                       |
| getUTCSeconds()                  | 返回UTC 日期中的秒（0~59）                                   |
| setSeconds(seconds)              | 设置日期中的秒（如果seconds 大于59，则加分）                 |
| setUTCSeconds(seconds)           | 设置UTC 日期中的秒（如果seconds 大于59，则加分）             |
| getMilliseconds()                | 返回日期中的毫秒                                             |
| getUTCMilliseconds()             | 返回UTC 日期中的毫秒                                         |
| setMilliseconds(milliseconds)    | 设置日期中的毫秒                                             |
| setUTCMilliseconds(milliseconds) | 设置UTC 日期中的毫秒                                         |
| getTimezoneOffset()              | 返回以分钟计的UTC 与本地时区的偏移量（如美国EST 即“东部标准时间”返回300，进入夏令时的地区可能有所差异） |



# RegExp

`ECMAScript` 通过`RegExp` 类型支持正则表达式。正则表达式使用类似`Perl` 的简洁语法来创建：

```js
let expression = /pattern/flags;
```



# 原始值包装类型

为了方便操作原始值，`ECMAScript` 提供了3 种特殊的引用类型：`Boolean`、`Number` 和`String`。

这些类型具有本章介绍的其他引用类型一样的特点，但也具有与各自原始类型对应的特殊行为。**每当用到某个原始值的方法或属性时，后台都会创建一个相应原始包装类型的对象，从而暴露出操作原始值的各种方法**。来看下面的例子：

```js
let s1 = "some text"; 
let s2 = s1.substring(2);
```



在这里，`s1` 是一个包含字符串的变量，它是一个原始值。第二行紧接着在`s1` 上调用了`substring()`方法，并把结果保存在`s2` 中。我们知道，原始值本身不是对象，因此逻辑上不应该有方法。而实际上这个例子又确实按照预期运行了。这是因为后台进行了很多处理，从而实现了上述操作。具体来说，当第二行访问`s1` 时，是以读模式访问的，也就是要从内存中读取变量保存的值。在以读模式访问字符串值的任何时候，后台都会执行以下3 步：

1. 创建一个`String` 类型的实例；
2. 调用实例上的特定方法；
3. 销毁实例。



可以把这3 步想象成执行了如下3 行`ECMAScript` 代码：

```js
let s1 = new String("some text"); 
let s2 = s1.substring(2); 

s1 = null; 
```

这种行为可以让原始值拥有对象的行为。对布尔值和数值而言，以上3 步也会在后台发生，只不过使用的是`Boolean` 和`Number` 包装类型而已。 

**引用类型与原始值包装类型的主要区别在于对象的生命周期**。在通过`new` 实例化引用类型后，得到的实例会在离开作用域时被销毁。



`Object` 构造函数作为一个工厂方法，能够根据传入值的类型返回相应原始值包装类型的实例。比如：

```js
let obj = new Object("some text"); 
console.log(obj instanceof String);  // true 
```

如果传给`Object` 的是字符串，则会创建一个`String` 的实例。如果是数值，则会创建`Number` 的实例。布尔值则会得到`Boolean` 的实例。



注意，使用`new` 调用原始值包装类型的构造函数，与调用同名的转型函数并不一样。例如：

```js
let value = "25"; 
let number = Number(value);    // 转型函数 
console.log(typeof number);    // "number" 
let obj = new Number(value);   // 构造函数 
console.log(typeof obj);       // "object"
```

在这个例子中，变量 `number` 中保存的是一个值为 25 的**原始数值**，而变量 `obj` 中保存的是一个`Number` 的实例。



## Boolean

`Boolean` 是对应布尔值的引用类型。要创建一个`Boolean` 对象，就使用`Boolean` 构造函数并传入`true` 或`false`，如下例所示：

```js
let booleanObject = new Boolean(true); 
```



`Boolean` 的实例会重写`valueOf()`方法，返回一个原始值`true` 或`false`。`toString()`方法被调用时也会被覆盖，返回字符串"true"或"false"。不过，`Boolean` 对象在 `ECMAScript` 中用得很少。**不仅如此，它们还容易引起误会，尤其是在布尔表达式中使用`Boolean` 对象时**，比如：

```js
let falseObject = new Boolean(false); 
let result = falseObject && true; 
console.log(result); // true 
 

let falseValue = false; 
result = falseValue && true; 
console.log(result); // false
```



在这段代码中，我们创建一个值为`false` 的`Boolean` 对象。然后，在一个布尔表达式中通过`&&`操作将这个对象与一个原始值`true` 组合起来。在布尔算术中，`false && true` 等于`false`。可是，这个表达式是对 `falseObject` 对象而不是对它表示的值（false）求值。前面刚刚说过，所有对象在布尔表达式中都会自动转换为`true`，因此 `falseObject`在这个表达式里实际上表示一个`true` 值。那么`true && true` 当然是`true`。



除此之外，原始值和引用值（`Boolean` 对象）还有几个区别。首先，`typeof` 操作符对原始值返回"boolean"，但对引用值返回"object"。同样，`Boolean` 对象是 `Boolean` 类型的实例，在使用`instaceof` 操作符时返回`true`，但对原始值则返回`false`，如下所示：

```js
console.log(typeof falseObject);             // object 
console.log(typeof falseValue);              // boolean 
console.log(falseObject instanceof Boolean); // true 
console.log(falseValue instanceof Boolean);  // false 
```

理解原始布尔值和`Boolean` 对象之间的区别非常重要，**强烈建议永远不要使用后者**。



## Number

`Number` 是对应数值的引用类型。要创建一个 `Number` 对象，就使用 `Number` 构造函数并传入一个 数值，如下例所示：

```js
let numberObject = new Number(10); 
```

与 `Boolean` 类型一样，`Number` 类型重写了 `valueOf()`、`toLocaleString()`和 `toString()`方 法。valueOf()方法返回`Number` 对象表示的原始数值，另外两个方法返回数值字符串。`toString()` 方法可选地接收一个表示基数的参数并返回相应基数形式的数值字符串，如下所示：

```js
let num = 10; 
console.log(num.toString()); // "10" 
console.log(num.toString(2)); // "1010" 
console.log(num.toString(8)); // "12" 
console.log(num.toString(10)); // "10" 
console.log(num.toString(16)); // "a" 
```

除了继承的方法，`Number` 类型还提供了几个用于将数值格式化为字符串的方法。

`toFixed()`方法返回包含指定小数点位数的数值字符串，如：

```js
let num = 10; 
console.log(num.toFixed(2)); // "10.00" 
```

这里的 `toFixed()`方法接收了参数 2，表示返回的数值字符串要包含两位小数。结果返回值为 "10.00"，小数位填充了 0。如果数值本身的小数位超过了参数指定的位数，则四舍五入到最接近的小数位：

```js
let num = 10.005; 
console.log(num.toFixed(2)); // "10.01" 
```

`toFixed()`自动舍入的特点可以用于处理货币。不过要注意的是，多个浮点数值的数学计算不一定 得到精确的结果。比如，0.1 + 0.2 = 0.30000000000000004。

另一个用于格式化数值的方法是 `toExponential()`，返回以科学记数法（也称为指数记数法）表 示的数值字符串。与 `toFixed()`一样，`toExponential()`也接收一个参数，表示结果中小数的位数。 来看下面的例子:

```js
let num = 10; 
console.log(num.toExponential(1)); // "1.0e+1"
```



与 `Boolean` 对象类似，`Number` 对象也为数值提供了重要能力。但是，考虑到两者存在同样的潜在问题，因此并不建议直接实例化 `Number` 对象。在处理原始数值和引用数值时，`typeof` 和 `instacnceof` 操作符会返回不同的结果，如下所示：

```js
let numberObject = new Number(10); 
let numberValue = 10; 
console.log(typeof numberObject); // "object" 
console.log(typeof numberValue); // "number" 
console.log(numberObject instanceof Number); // true 
console.log(numberValue instanceof Number); // false 
```

原始数值在调用 `typeof` 时始终返回"number"，而 `Number` 对象则返回"object"。类似地，`Number` 对象是 `Number` 类型的实例，而原始数值不是。



`isInteger()`方法与安全整数

`ES6` 新增了 `Number.isInteger()`方法，用于辨别一个数值是否保存为整数。有时候，小数位的 0 可能会让人误以为数值是一个浮点值：

```js
console.log(Number.isInteger(1)); // true 
console.log(Number.isInteger(1.00)); // true 
console.log(Number.isInteger(1.01)); // false 
```



`IEEE 754` 数值格式有一个特殊的数值范围，在这个范围内二进制值可以表示一个整数值。这个数值 范围从 `Number.MIN_SAFE_INTEGER`（-2<sup>53</sup>+ 1）到 `Number.MAX_SAFE_INTEGER`（2<sup>53</sup> - 1）。对超出这 个范围的数值，即使尝试保存为整数，`IEEE 754` 编码格式也意味着二进制值可能会表示一个完全不同的 数值。为了鉴别整数是否在这个范围内，可以使用 `Number.isSafeInteger()`方法：

```js
console.log(Number.isSafeInteger(-1 * (2 ** 53))); // false 
console.log(Number.isSafeInteger(-1 * (2 ** 53) + 1)); // true 
console.log(Number.isSafeInteger(2 ** 53)); // false 
console.log(Number.isSafeInteger((2 ** 53) - 1)); // true 
```



## String

`String` 是对应字符串的引用类型。要创建一个 `String` 对象，使用 `String`构造函数并传入一个 数值，如下例所示：

```js
let stringObject = new String("hello world");
```

`String` 对象的方法可以在所有字符串原始值上调用。3个继承的方法 `valueOf()`、`toLocaleString()` 和 `toString()`都返回对象的原始字符串值。



每个 `String` 对象都有一个 `length` 属性，表示字符串中字符的数量。来看下面的例子：

```js
let stringValue = "hello world"; 
console.log(stringValue.length); // "11" 
```

注意，即使字符串中包含双字节 字符（而不是单字节的 ASCII 字符），也仍然会按单字符来计数。



1. JavaScript字符

   `JavaScript`  字符串由 16 位码元（code unit）组成。对多数字符来说，每 16 位码元对应一个字符。换句话说，字符串的 `length` 属性表示字符串包含多少 16 位码元。

   

   此外，`charAt()`方法返回给定索引位置的字符，由传给方法的整数参数指定。具体来说，这个方法查找指定索引位置的 16 位码元，并返回该码元对应的字符：

   ```js
   let message = "abcde"; 
   console.log(message.charAt(2)); // "c" 
   ```

   `JavaScript` 字符串使用了两种 `Unicode` 编码混合的策略：`UCS-2` 和 `UTF-16`。对于可以采用 16 位编码 的字符（U+0000~U+FFFF），这两种编码实际上是一样的。

   

   使用 `charCodeAt()`方法可以查看指定码元的字符编码。这个方法返回指定索引位置的码元值，索 引以整数指定。比如：

   ```js
   let message = "abcde"; 
   // Unicode "Latin small letter C"的编码是 U+0063 
   console.log(message.charCodeAt(2)); // 99 
   // 十进制 99 等于十六进制 63 
   console.log(99 === 0x63); // true
   ```

2. 字符串操作方法

   首先是 `concat()`，用于将一个或多个字符串拼接成一个新字符串。来看下面的例子：

   ```js
   let stringValue = "hello "; 
   let result = stringValue.concat("world"); 
   console.log(result); // "hello world" 
   console.log(stringValue); // "hello"
   ```

   对 `stringValue` 调 用 `concat()`方法的结果是得到"hello world"， 但 `stringValue` 的值保持不变。`concat()`方法可以接收任意多个参数，因此可以一次性拼接多个字符串， 如下所示：

   ```js
   let stringValue = "hello "; 
   let result = stringValue.concat("world", "!"); 
   console.log(result); // "hello world!" 
   console.log(stringValue); // "hello" 
   ```

   虽然 `concat()`方法可以拼接 字符串，但更常用的方式是使用加号操作符（+）。而且多数情况下，对于拼接多个字符串来说，使用加号更方便。

   

   `ECMAScript` 提供了 3 个从字符串中提取子字符串的方法：`slice()`、`substr()`和 `substring()`。3个方法都返回调用它们的字符串的一个子字符串，而且都接收一或两个参数。**第一个参数表示子字符串开始的位置，第二个参数表示子字符串结束的位置**。对 `slice()`和 `substring()`而言，第二个参数是提取结束的位置（即该位置之前的字符会被提取出来）。对 `substr()`而言，第二个参数表示返回的子字符串数量。 **任何情况下，省略第二个参数都意味着提取到字符串末尾**。与 `concat()`方法一样，`slice()`、`substr()` 和 `substring()`也不会修改调用它们的字符串，而只会返回提取到的原始新字符串值。来看下面的例子：

   ```js
   let stringValue = "hello world"; 
   console.log(stringValue.slice(3)); // "lo world" 
   console.log(stringValue.substring(3)); // "lo world" 
   console.log(stringValue.substr(3)); // "lo world" 
   console.log(stringValue.slice(3, 7)); // "lo w" 
   console.log(stringValue.substring(3,7)); // "lo w" 
   console.log(stringValue.substr(3, 7)); // "lo worl" 
   ```

   当某个参数是负值时，这 3 个方法的行为又有不同。比如，`slice()`方法将所有负值参数都当成字符串长度加上负参数值。而 `substr()`方法将第一个负参数值当成字符串长度加上该值，将第二个负参数值转换为 0。 substring()方法会将所有负参数值都转换为 0。看下面的例子：

   ```js
   let stringValue = "hello world"; 
   console.log(stringValue.slice(-3)); // "rld" 
   console.log(stringValue.substring(-3)); // "hello world" 
   console.log(stringValue.substr(-3)); // "rld" 
   console.log(stringValue.slice(3, -4)); // "lo w" 
   console.log(stringValue.substring(3, -4)); // "hel" 
   console.log(stringValue.substr(3, -4)); // "" (empty string) 
   ```

3. 字符串位置方法

   有两个方法用于在字符串中定位子字符串：`indexOf()`和 `lastIndexOf()`。这两个方法从字符 串中搜索传入的字符串，并返回位置（如果没找到，则返回-1）。两者的区别在于，`indexOf()`方法**从字符串开头开始查找子字符串**，而 `lastIndexOf()`方法**从字符串末尾开始查找子字符串**。来看下面的例子：

   ```js
   let stringValue = "hello world"; 
   console.log(stringValue.indexOf("o")); // 4 
   console.log(stringValue.lastIndexOf("o")); // 7 
   ```

   **这两个方法都可以接收可选的第二个参数，表示开始搜索的位置**。这意味着，`indexOf()`会从这个 参数指定的位置开始向字符串末尾搜索，忽略该位置之前的字符；`lastIndexOf()`则会从这个参数指的位置开始向字符串开头搜索，忽略该位置之后直到字符串末尾的字符。下面看一个例子：

   ```js
   let stringValue = "hello world"; 
   console.log(stringValue.indexOf("o", 6)); // 7 
   console.log(stringValue.lastIndexOf("o", 6)); // 4 
   ```

   像这样使用第二个参数并循环调用 `indexOf()`或 `lastIndexOf()`，就可以在字符串中找到所有的目标子字符串，如下所示：

   ```js
   let stringValue = "Lorem ipsum dolor sit amet, consectetur adipisicing elit"; 
   let positions = new Array(); 
   let pos = stringValue.indexOf("e"); 
   while(pos > -1) { 
     positions.push(pos); 
     pos = stringValue.indexOf("e", pos + 1); 
   } 
   console.log(positions); // [3,24,32,35,52] 
   ```

4. 字符串包含方法

   `ECMAScript` 6 增加了 3 个用于判断字符串中是否包含另一个字符串的方法：`startsWith()`、 `endsWith()`和 `includes()`。这些方法都会从字符串中搜索传入的字符串，并返回一个表示是否包含 的布尔值。它们的区别在于，`startsWith()`检查开始于索引 0 的匹配项，`endsWith()`检查开始于索 引(`string.length` - `substring.length`)的匹配项，而 `includes()`检查整个字符串：

   ```js
   let message = "foobarbaz"; 
   console.log(message.startsWith("foo")); // true 
   console.log(message.startsWith("bar")); // false 
   console.log(message.endsWith("baz")); // true 
   console.log(message.endsWith("bar")); // false 
   console.log(message.includes("bar")); // true 
   console.log(message.includes("qux")); // false
   ```

   `startsWith()`和 `includes()`方法接收可选的第二个参数，表示开始搜索的位置。如果传入第二个参数，则意味着这两个方法会从指定位置向着字符串末尾搜索，忽略该位置之前的所有字符。

   

   `endsWith()`方法接收可选的第二个参数，表示应该当作字符串末尾的位置。如果不提供这个参数，那么默认就是字符串长度。如果提供这个参数，那么就好像字符串只有那么多字符一样： 

   ```js
   let message = "foobarbaz"; 
   
   console.log(message.endsWith("bar"));     // false 
   console.log(message.endsWith("bar", 6));  // true
   ```

5. trim()方法

   `ECMAScript` 在所有字符串上都提供了trim()方法。这个方法会创建字符串的一个副本，删除前、后所有空格符，再返回结果。比如：

   ```js
   let stringValue = "  hello world  "; 
   
   let trimmedStringValue = stringValue.trim(); 
   console.log(stringValue);         // "  hello world " 
   console.log(trimmedStringValue);  // "hello world"
   ```

   由于`trim()`返回的是字符串的副本，因此原始字符串不受影响，即原本的前、后空格符都会保留。 另外，`trimLeft()`和`trimRight()`方法分别用于从字符串开始和末尾清理空格符。

6. repeat()方法 

   `ECMAScript` 在所有字符串上都提供了 `repeat()`方法。这个方法接收一个整数参数，表示要将字符串复制多少次，然后返回拼接所有副本后的结果。

   ```js
   let stringValue = "na "; 
   console.log(stringValue.repeat(16) + "batman"); 
   // na na na na na na na na na na na na na na na na batman
   ```

7. padStart()和padEnd()方法

   `padStart()`和 `padEnd()`方法会复制字符串，如果小于指定长度，则在相应一边填充字符，直至满足长度条件。这两个方法的第一个参数是长度，第二个参数是可选的填充字符串，默认为空格（U+0020）。

   ```js
   let stringValue = "foo"; 
    
   console.log(stringValue.padStart(6));       // "   foo" 
   console.log(stringValue.padStart(9, "."));  // "......foo" 
    
   console.log(stringValue.padEnd(6));         // "foo   " 
   console.log(stringValue.padEnd(9, "."));    // "foo......"
   ```

   可选的第二个参数并不限于一个字符。如果提供了多个字符的字符串，则会将其拼接并截断以匹配指定长度。此外，如果长度小于或等于字符串长度，则会返回原始字符串。

   

   ```js
   let stringValue = "foo"; 
    
   console.log(stringValue.padStart(8, "bar")); // "barbafoo" 
   
   console.log(stringValue.padStart(2));        // "foo" 
    
   console.log(stringValue.padEnd(8, "bar"));   // "foobarba" 
   console.log(stringValue.padEnd(2));          // "foo" 
   ```

8. 字符串迭代与解构

   字符串的原型上暴露了一个`@@iterator` 方法，表示可以迭代字符串的每个字符。可以像下面这样手动使用迭代器：

   ```js
   let message = "abc"; 
   let stringIterator = message[Symbol.iterator](); 
   
   console.log(stringIterator.next());  // {value: "a", done: false} 
   console.log(stringIterator.next());  // {value: "b", done: false} 
   console.log(stringIterator.next());  // {value: "c", done: false} 
   console.log(stringIterator.next());  // {value: undefined, done: true}
   ```

   在`for-of` 循环中可以通过这个迭代器按序访问每个字符：

   ```js
   for (const c of "abcde") { 
     console.log(c); 
   }
   ```

   ```js
   // a 
   // b 
   // c 
   // d 
   // e
   ```

   有了这个迭代器之后，字符串就可以通过解构操作符来解构了。比如，可以更方便地把字符串分割为字符数组：

   ```js
   let message = "abcde"; 
   console.log([...message]); // ["a", "b", "c", "d", "e"]
   ```

9. 字符串大小写转换 

   下一组方法涉及大小写转换，包括 4 个方法：`toLowerCase()`、`toLocaleLowerCase()`、`toUpperCase()`和`toLocaleUpperCase()`。`toLowerCase()`和`toUpperCase()`方法是原来就有的方法，与`java.lang.String` 中的方法同名。`toLocaleLowerCase()`和`toLocaleUpperCase()`方法旨在基于特定地区实现。在很多地区，地区特定的方法与通用的方法是一样的。但在少数语言中（如土耳其语），`Unicode` 大小写转换需应用特殊规则，要使用地区特定的方法才能实现正确转换。下面是几个例子：

   ```js
   let stringValue = "hello world"; 
   console.log(stringValue.toLocaleUpperCase());  // "HELLO WORLD" 
   console.log(stringValue.toUpperCase());        // "HELLO WORLD" 
   console.log(stringValue.toLocaleLowerCase());  // "hello world" 
   console.log(stringValue.toLowerCase());        // "hello world" 
   ```

10. 字符串模式匹配方法

    `String` 类型专门为在字符串中实现模式匹配设计了几个方法。第一个就是`match()`方法，这个方法本质上跟`RegExp` 对象的 `exec()`方法相同。`match()`方法接收一个参数，可以是一个正则表达式字符串，也可以是一个`RegExp` 对象。来看下面的例子：

    ```js
    let text = "cat, bat, sat, fat"; 
    let pattern = /.at/; 
     
    // 等价于pattern.exec(text) 
    let matches = text.match(pattern); 
    console.log(matches.index);      // 0 
    console.log(matches[0]);         // "cat" 
    console.log(pattern.lastIndex);  // 0
    ```

    另一个查找模式的字符串方法是`search()`。这个方法唯一的参数与`match()`方法一样：正则表达式字符串或`RegExp` 对象。这个方法返回模式第一个匹配的位置索引，如果没找到则返回-1。`search()`始终从字符串开头向后匹配模式。看下面的例子：

    ```js
    let text = "cat, bat, sat, fat"; 
    let pos = text.search(/at/); 
    console.log(pos);  // 1
    ```

    这里，`search(/at/)`返回1，即"at"的第一个字符在字符串中的位置。

    

    为简化子字符串替换操作，`ECMAScript` 提供了`replace()`方法。这个方法接收两个参数，第一个参数可以是一个 `RegExp` 对象或一个字符串（这个字符串不会转换为正则表达式），第二个参数可以是一个字符串或一个函数。如果第一个参数是字符串，那么只会替换第一个子字符串。要想替换所有子字符串，第一个参数必须为正则表达式并且带全局标记，如下面的例子所示： 

    ```js
    let text = "cat, bat, sat, fat"; 
    let result = text.replace("at", "ond"); 
    console.log(result);  // "cond, bat, sat, fat" 
    
     
    result = text.replace(/at/g, "ond"); 
    console.log(result);  // "cond, bond, sond, fond"
    ```

11. localeCompare()方法 

    最后一个方法是`localeCompare()`，这个方法比较两个字符串，返回如下3 个值中的一个。

    1. 如果按照字母表顺序，字符串应该排在字符串参数前头，则返回负值。（通常是-1，具体还要看与实际值相关的实现。） 
    2. 如果字符串与字符串参数相等，则返回0。 
    3. 如果按照字母表顺序，字符串应该排在字符串参数后头，则返回正值。（通常是 1，具体还要看与实际值相关的实现。）

    ```js
    let stringValue = "yellow"; 
    console.log(stringValue.localeCompare("brick"));  // 1 
    console.log(stringValue.localeCompare("yellow")); // 0 
    console.log(stringValue.localeCompare("zoo"));    // -1
    ```

    `localeCompare()`的独特之处在于，实现所在的地区（国家和语言）决定了这个方法如何比较字符串。在美国，英语是`ECMAScript` 实现的标准语言，`localeCompare()`区分大小写，大写字母排在小写字母前面。但其他地区未必是这种情况。



# 单例内置对象

`ECMA-262` 对内置对象的定义是“任何由`ECMAScript` 实现提供、与宿主环境无关，并在`ECMAScript`程序开始执行时就存在的对象”。这就意味着，开发者不用显式地实例化内置对象，因为它们已经实例化好了。前面我们已经接触了大部分内置对象，包括`Object`、`Array` 和`String`。本节介绍`ECMA-262`定义的另外两个单例内置对象：`Global` 和`Math`。



## Global

`Global` 对象是`ECMAScript` 中最特别的对象，因为代码不会显式地访问它。`ECMA-262` 规定`Global`对象为一种兜底对象，它所针对的是不属于任何对象的属性和方法。事实上，**不存在全局变量或全局函数这种东西。在全局作用域中定义的变量和函数都会变成`Global` 对象的属性** 。前面介绍的函数，包括`isNaN()`、`isFinite()`、`parseInt()`和`parseFloat()`，实际上都是`Global` 对象的方法。除了这些，`Global` 对象上还有另外一些方法。



1. URL编码方法

   `encodeURI()`和`encodeURIComponent()`方法用于编码统一资源标识符（URI），以便传给浏览器。有效的URI 不能包含某些字符，比如空格。使用URI 编码方法来编码URI 可以让浏览器能够理解它们，同时又以特殊的`UTF-8` 编码替换掉所有无效字符。

   `ecnodeURI()`方法用于对整个 URI 进行编码，比如"www.wrox.com/illegal value.js"。而`encodeURIComponent()`方法用于编码URI 中单独的组件，比如前面URL 中的"illegal value.js"。

   

   这两个方法的主要区别是，`encodeURI()`不会编码属于URL 组件的特殊字符，比如冒号、斜杠、问号、井号，而`encodeURIComponent()`会编码它发现的所有非标准字符。来看下面的例子：

   ```js
   let uri = "http://www.wrox.com/illegal value.js#start"; 
    
   // "http://www.wrox.com/illegal%20value.js#start" 
   console.log(encodeURI(uri)); 
   
   // "http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.js%23start" 
   console.log(encodeURIComponent(uri)); 
   ```

   与`encodeURI()`和`encodeURIComponent()`相对的是`decodeURI()`和`decodeURIComponent()`。`decodeURI()`只对使用`encodeURI()`编码过的字符解码。例如，%20 会被替换为空格，但%23 不会被替换为井号（#），因为井号不是由`encodeURI()`替换的。类似地，`decodeURIComponent()`解码所有被`encodeURIComponent()`编码的字符，基本上就是解码所有特殊值。

2. eval()方法

   最后一个方法可能是整个 `ECMAScript` 语言中最强大的了，它就是 `eval()`。这个方法就是一个完整的 `ECMAScript` 解释器，它接收一个参数，即一个要执行的 `ECMAScript`（`JavaScript`）字符串。来看一个例子：

   ```js
   eval("console.log('hi')");
   ```

   上面这行代码的功能与下一行等价：

   ```js
   console.log("hi");
   ```

   当解释器发现`eval()`调用时，会将参数解释为实际的`ECMAScript` 语句，然后将其插入到该位置。通过 `eval()`执行的代码属于该调用所在上下文，被执行的代码与该上下文拥有相同的作用域链。这意味着定义在包含上下文中的变量可以在`eval()`调用内部被引用，比如下面这个例子： 

   ```js
   let msg = "hello world"; 
   eval("console.log(msg)");  // "hello world"
   ```

   类似地，可以在`eval()`内部定义一个函数或变量，然后在外部代码中引用，如下所示：

   ```js
   eval("function sayHi() { console.log('hi'); }"); 
   sayHi();
   ```

   通过 `eval()`定义的任何变量和函数都不会被提升，这是因为在解析代码的时候，它们是被包含在一个字符串中的。它们只是在`eval()`执行的时候才会被创建。

   在严格模式下，在 `eval()`内部创建的变量和函数无法被外部访问。换句话说，最后两个例子会报错。同样，在严格模式下，赋值给`eval` 也会导致错误：

   ```js
   "use strict"; 
   eval = "hi";  // 导致错误
   ```

3. windows对象

   虽然`ECMA-262`没有规定直接访问`Global` 对象的方式，但浏览器将`window` 对象实现为`Global`对象的代理。因此，所有全局作用域中声明的变量和函数都变成了`window` 的属性。来看下面的例子：

   ```js
   var color = "red"; 
    
   function sayColor() { 
     console.log(window.color); 
   } 
    
   window.sayColor(); // "red" 
   ```

   这里定义了一个名为color的全局变量和一个名为`sayColor()`的全局函数。在`sayColor()`内部，通过`window.color` 访问了`color` 变量，说明全局变量变成了`window` 的属性。接着，又通过`window`对象直接调用了`window.sayColor()`函数，从而输出字符串。 

   

   另一种获取Global 对象的方式是使用如下的代码：

   ```js
   let global = function() { 
     return this;  
   }();
   ```

   这段代码创建一个立即调用的函数表达式，返回了`this` 的值。如前所述，当一个函数在没有明确（通过成为某个对象的方法，或者通过`call()/apply()）`指定`this` 值的情况下执行时，`this` 值等于`Global` 对象。因此，调用一个简单返回`this` 的函数是在任何执行上下文中获取`Global` 对象的通用方式。 



## Math

`ECMAScript` 提供了 `Math` 对象作为保存数学公式、信息和计算的地方。`Math` 对象提供了一些辅助计算的属性和方法。



1. min()和 max()方法

   `Math` 对象提供了很多辅助执行简单或复杂数学计算的方法。 `min()`和 `max()`方法用于确定一组数值中的最小值和最大值。这两个方法都接收任意多个参数，如下面的例子所示：

   ```json
   let max = Math.max(3, 54, 32, 16); 
   console.log(max); // 54 
   let min = Math.min(3, 54, 32, 16); 
   console.log(min); // 3 
   ```

   要知道数组中的最大值和最小值，可以像下面这样使用扩展操作符：

   ```js
   let values = [1, 2, 3, 4, 5, 6, 7, 8]; 
   let max = Math.max(...val);
   ```

2. 舍入方法

   接下来是用于把小数值舍入为整数的 4 个方法：`Math.ceil()`、`Math.floor()`、`Math.round() `和 `Math.fround()`。这几个方法处理舍入的方式如下所述。

   - `Math.ceil()`方法始终向上舍入为最接近的整数。
   - `Math.floor()`方法始终向下舍入为最接近的整数。
   - `Math.round()`方法执行四舍五入。
   - `Math.fround()`方法返回数值最接近的单精度（32 位）浮点值表示。

3. random()方法

   `Math.random()`方法返回一个 0~1 范围内的随机数，其中包含 0 但不包含 1。对于希望显示随机名言或随机新闻的网页，这个方法是非常方便的。可以基于如下公式使用 `Math.random()`从一组整数中随机选择一个数：

   ```js
   number = Math.floor(Math.random() * total_number_of_choices + first_possible_value) 
   ```

   这里使用了 `Math.floor()`方法，因为 `Math.random()`始终返回小数，即便乘以一个数再加上一 个数也是小数。因此，如果想从 1~10 范围内随机选择一个数，代码就是这样的：

   ```js
   let num = Math.floor(Math.random() * 10 + 1);
   ```

   这样就有 10 个可能的值（1~10），其中最小的值是 1。如果想选择一个 2~10 范围内的值，则代码就要写成这样：

   ```js
   let num = Math.floor(Math.random() * 9 + 2); 
   ```

   2~10 只有 9 个数，所以可选总数（total_number_of_choices）是 9，而最小可能的值 （first_possible_value）是 2。很多时候，通过函数来算出可选总数和最小可能的值可能更方便， 比如：

   ```js
   function selectFrom(lowerValue, upperValue) { 
        let choices = upperValue - lowerValue + 1; 
        return Math.floor(Math.random() * choices + lowerValue); 
   } 
   let num = selectFrom(2,10); 
   console.log(num); // 2~10 范围内的值，其中包含 2 和 10
   ```

   这里的函数 `selectFrom()`接收两个参数：应该返回的最小值和最大值。通过将这两个值相减再加 1 得到可选总数，然后再套用上面的公式。于是，调用 `selectFrom(2,10)`就可以从 2~10（包含） 范围内选择一个值了。



