---
title: 克服 Javascript 的奇怪部分 動態型別
date: 2022-09-23 15:31:26
tags:
- Javascript
categories: javascript
---

運算過程中由於 Javascript 是弱型別，沒有辦法強制規範型別，所以會在運算過程中發生隱式（動態）型別轉換，稱之為 Corecion。
{% blockquote %}
Converting a value from one type to another.
{% endblockquote %}

# 算術運算子的隱式轉換
{% codeblock lang:JavaScript %}
const a = 1 + '2';
console.log(a); // 12
{% endcodeblock %}
上方範例可以印證 Javascript 引擎將數字 `1` 與字串 `2` 相加的過程中產生的動態型別的轉換，獲得了字串 `12`。
在某些強型別的語言中，這樣做是會噴 error 的，但 Javascript 不會，這也造成開發過程時常會發生不如預期的結果。

# 比較運算子的隱式轉換
從以下的範例我們可以很自然地獲得 true 的結果：
{% codeblock lang:JavaScript %}
const a = 3 > 2 > 1;
console.log(a); // true
{% endcodeblock %}
然而以下的範例卻出乎意料之外也獲得了 true：
{% codeblock lang:JavaScript %}
const a = 3 < 2 < 1;
console.log(a); // true
{% endcodeblock %}
這也是動態型別的轉換在作怪，一一拆解一下過程是這樣的，比較運算子 `<` 在前一篇的相依性有提到：
{% img /images/js-weird-parts-coercion/0.png 800 200 js-weird-parts-coercion %}
當運算子相同時會採用相依性的方向來決定計算次序，比較運算子 `<` 是左相依性（left-to-right associactivity），
所以背後的 Javascript 引擎會如是計算：
`3 < 2` 獲得 false，`false < 1` 由於型別不同 Javascript 引擎強制將 false 使用數字包裹器轉換為數字型別：
{% codeblock lang:JavaScript %}
console.log(Number(false)); // 0
{% endcodeblock %}
`0 < 1` 獲得 true。

# 強制型轉
型轉分為主動式強制型轉（Explicit Coercion）與被動式隱式型轉（Implicit Coercion），強制型轉有幾個基本型：
1. toString
2. toNumber
3. toBoolean
4. toPrimitive

### toNumber 數字型轉範例
以下是幾個特殊的案例：
{% codeblock lang:JavaScript %}
Number(Undefined); // NaN
Number(null); // 0
{% endcodeblock %}


### 雙等號型轉的悲劇
雙等號（double Equality）會造成嚴重的動態型別轉換，引發不可預期的後果，例如以下的範例：
{% codeblock lang:JavaScript %}
false == 0; // true
null == 0; // true
null < 1; // true
"" == 0; // true
"" < 1; // true
{% endcodeblock %}

#### 動態型別呼叫的包裹器？
Javascript 引擎在型別轉換背後做了許多事情，但也非沒有規則可循，
以下的範例是 JavaScript 最難以理解的一部分型別轉換：

##### 比較運算子呼叫 toString
{% codeblock lang:JavaScript %}
[] == false; // true
Boolean([]); // true
{% endcodeblock %}

這實在是太詭異了!!不過理解一下背後的原理，「當 Array 拿去比較 value 的時候，toString 包裹器會被呼叫，而不是透過　Boolean 轉換」：
{% codeblock lang:JavaScript %}
String([]); // ''
'' == false; // true
{% endcodeblock %}

### 有趣的 [object object]
在開發過程查詢 error 或其他型轉情境經常會看見或 alert 噴出 `[object object]` 這類特殊的值，剖析一下出現的原理：

#### 型轉造成的 [object object]
{% codeblock lang:JavaScript %}
[] + {}; // '[object object]'
{} + []; // 0
{% endcodeblock %}
第一個例子 `[] + {};`，這是由於 `[]` + 號型轉為空字串 `''`，而 `{}` + 號型轉為 '[object object]' 了；String() 直接調用 `.toString` 方法來轉換 `[]`。

第二個例子 `{} + []` 中， `{}`被當作空區塊無作用，只會運算後方的 ` + []` 調用 `Number([])`，
依據型轉規則，陣列屬於物件型別所以型轉數字時會調用 toString 方法來型轉成空字串 `String([])` 獲得 `''`，也就是 Number('')，
而獲得 0 的結果。
{% colorquote danger %}
值得注意的是 block `{}` 放置在最前方會被為 Javascript 引擎視作作用域而非空物件!
{% endcolorquote %}

### 善用嚴格等式（Tripple Equality）
不同於雙等號造成的型轉悲劇，三等號可以確保最後獲得的比較結果不會被型轉，也能達到預期的值做開發判斷，是比較良好的開發習慣。