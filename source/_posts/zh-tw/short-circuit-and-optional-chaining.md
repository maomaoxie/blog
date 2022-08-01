---
title: 短路解析 & 可選鍊修飾符
date: 2022-03-12 13:55:02
tags:
- shortCircuit
- optionalChaining
- javascript
- tricks
categories: javascript
---
{% img /images/short-circuit-and-optional-chaining/0.png 800 200 短路解析與可選練修飾符 %}
在開發的情境上無論是串接 API 或者是資料判斷，都需要追求便捷而好懂的方式來維護程式碼，以利於當資料判斷變得複雜臃腫時仍可以邏輯清晰。而對於程式新手來說`undefined`（未定義）、`null`（空值）或者是`0`（零）在判斷上是很容易掉進去的陷阱，因為判定的方法了解的不深而陷入困境。

# 理解真假值
先介紹 truthy(真值) 與 falsy(假值)：
- truthy：非 falsy 的值，或者是表達式結果為 `true`
- falsy：`undefined`、`null`、非數字 `NaN`、數字 `0`、數字 `-0`、BigInt `0n`、空字串`''`(字串長度為 0)，或者是表達式結果為 `false`

以下介紹幾種邏輯判斷的捷徑：

# 邏輯運算子 `&&` `||`
舉個栗子
{% codeblock 邏輯運算子 lang:JavaScript %}
  const numberAND = 6
  const numberOR = -1
  if(numberAND > 5 && numberAND < 7) {
    console.log('哎呀')
  }
  if(numberOR > 5 || numberOR < 7) {
    console.log('黑唷')
  }
{% endcodeblock %}

### 人類的理解
當 numberAND 大於 5 且**同時**小於 7 值執行 `console.log('符合 &&')`；當 numberOR 大於 5 **或者**小於 7 值執行 `console.log('符合 ||')`

### 電腦的理解
當左邊的表達式 `numberAND > 5` 結果為 `false` 則跳過右邊不執行 `numberAND < 7`，直接**跳出**程式。
當左邊的表達式 `numberOR > 5` 結果為 `true` 則跳過右邊不執行 `numberAND < 7`，直接**進入**程式。

結論為，當左邊的表達式符合條件，`&&` 的 if 直接跳出；`||` 的 if 直接進入。
來理一理箇中原由吧！

# 短路解析(Short Circuit)
短路我個人覺得沒有捷徑來得好懂，短路比較讓人聯想為損毀或壞掉的電子產品，而捷徑則代表透過偷吃步或者抄捷徑的方式以取得一樣的結果。這裡的短路較接近後者，js 運行上的偷吃步：

### `&&` 的短路解析
當左邊的表達式為 `false` 就返回左邊的表達式結果，並且**直接忽視**右邊的表達式結果，反之執行右邊的表達式。

### `||` 的短路解析
當左邊的表達式為 `true` 就返回左邊的表達式結果，並且**直接忽視**右邊的表達式結果，反之執行右邊的表達式。

以上可以知道 js 執行完左邊的表達式之後，若符合條件則直接跳過右邊的表達式(不解析亦不執行)，相對來講當開發人員在理解短路解析時就可以按照這樣的邏輯去快速判斷結果。

速記法：AND`false`跳出、OR`true`進入(執行)。

{% blockquote %}
  if you use `||` to provide some default value to another variable foo, you may encounter unexpected behaviors if you consider some falsy values as usable (e.g., `''` or `0`). 
{% endblockquote %}

# 可選鍊修飾符(Optional chaining)
舉個栗子
{% codeblock 可選鍊修飾符 lang:JavaScript %}
let user = {};
console.log(user.address);
// undefined
console.log(user.address.street);
// error
console.log(user.address?.street);
// undefined
{% endcodeblock %}
在這個範例中，user 物件的清單中有部分的使用者缺少了 `address` 這個屬性，但是大部分的使用者都具有該屬性時，就可以使用可選鍊修飾符來避免程式噴錯而中斷，但是需注意避免**過度使用**可選鍊修飾符

# 空值合併運算子(Nullish coalescing operator) `??`
舉個栗子
{% codeblock 空值合併運算子 lang:JavaScript %}
  function notNullish (a, b) {
    console.log(a ?? b);
  }
  notNullish (undefined, null); // null
  notNullish (null, undefined); // undefined
  notNullish (undefined, 0); // 0
  notNullish (undefined, ''); // ''
{% endcodeblock %}

- ES2020（ES11）提供的「空值合併運算子」，支援度需要搭配 babel 套件。
- `??`常常與邏輯運算子 `||` 比較，前者返回非 `undefined` 與 `null` 的表達式，後者返回非 falsy 的表達式。過濾的條件上有區別，可以依照需求使用。

### `??` 的短路解析
當左邊的表達式為 非 `undefined` 與 `null` 就返回左邊的表達式結果，並且**直接忽視**右邊的表達式結果，反之執行右邊的表達式。


