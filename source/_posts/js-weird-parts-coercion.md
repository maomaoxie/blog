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

# 善用嚴格等式（triple equal）
雙等號會造成嚴重的動態型別轉換，引發不可預期的後果，例如以下的範例：
{% codeblock lang:JavaScript %}
false == 0; // true
null == 0; // true
null < 1; // true
"" == 0; // true
"" < 1; // true
{% endcodeblock %}
以下的範例是 JavaScript 最難以理解的一部分型別轉換：
{% codeblock lang:JavaScript %}
[] == false; // true
Boolean([]); // true
{% endcodeblock %}
這實在是太詭異了!!不過理解一下背後的原理，當 Array 拿去比較 value 的時候，toString 包裹器會被呼叫：
{% codeblock lang:JavaScript %}
String([]); // ''
'' == false; // true
{% endcodeblock %}