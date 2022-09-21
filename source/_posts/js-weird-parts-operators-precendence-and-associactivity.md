---
title: 克服 Javascript 的奇怪部分 運算子的相依性與優先性
date: 2022-09-21 14:58:56
tags:
categories:
---

在了解以下的特性以前需要知道，JavaScript 是 syncrounous 同步在執行一個表達式的，
所以一次只能執行一個片段的程式碼，也就是一個表達式，一個表達式只能包含一個運算子與兩個參數，所以需要決定執行的次序．

# 優先性（precendence）
{% blockquote %}
When there is more than one operators in one executable code, which operator will be called in order of precedence.
{% endblockquote %}
當某片段的執行碼具有多個運算子時，那個先執行取決於優先性（precendence）

{% codeblock lang:JavaScript %}
var a = 3 + 4 * 5;
console.log(a); // 23
{% endcodeblock %}

從以下圖表可以檢視優先性：
{% img /images/js-weird-parts-operators-precendence-and-associactivity/0.png 800 200 js-weird-parts-operators-precendence-and-associactivity %}
由於 * 運算子的優先性大於 + 運算子，所以 Javacript 會優先執行 `4 * 5` 爾後執行 `20 + 3`；
等號的優先性只有 2，所以會最後執行 `a = 23`。

# 相依性（Associativity）
{% blockquote %}
The Associativity is the percedence that determines the operators being called from left to right,
or right to left when the percedence are all the same.
{% endblockquote %}
當某片段程式碼中所有的運算子優先性相同時，由相依性來決定執行次序為左相依性還是右相依性：
左相依性（Left Associativity）表示由左到右執行；右相依性（Right Associativity）表示由右到左執行。

{% codeblock lang:JavaScript %}
var a = 2, b = 3, c = 4;
a = b = c;
console.log(a); // 4
console.log(b); // 4
console.log(c); // 4
{% endcodeblock %}

{% img /images/js-weird-parts-operators-precendence-and-associactivity/1.png 800 200 js-weird-parts-operators-precendence-and-associactivity %}
從上圖可以了解到 = 運算子的相依性是右相依性（right to left, Right Associativity），
當運算子的優先性都相同時 Javacript 會優先執行右邊的運算子，然後向左一個一個執行。
`a = b = c;` 表達式會先執行 `b = 4`，並且回傳 4 之後執行 `a = 4`。

# 大括號（parentheses）最優先
當一個表達式中具有多個運算子，大括號（parentheses）裡的運算會最優先：
{% codeblock lang:JavaScript %}
var a = (3 + 4) * 5;
console.log(a); // 35
{% endcodeblock %}
