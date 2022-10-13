---
title: 克服 Javascript 的奇怪部分 布林與存在
date: 2022-09-26 16:32:21
tags:
categories:
---

# 布林值的型轉
布林型轉遇到代表著空值、不存在或者未定義的值時就會轉換為假值（falsy），例如以下範例：
{% codeblock lang:JavaScript %}
Boolean(undefined); // false
Boolean(null); // false
Boolean(''); // false
Boolean(0); // false
Boolean(NaN); // false
{% endcodeblock %}

需要注意的是字串0使用布林型轉會獲得真值（truthy）：
{% codeblock lang:JavaScript %}
Boolean('0'); // true
Boolean('-0'); // true
{% endcodeblock %}

# if 陳述句的型轉
### 用來檢查某變數是否存在
if 陳述句會將大括號中（parenthesis）的值做布林轉換，也可以使用隱式型轉的特性來檢查某變數是否存在：
{% codeblock lang:JavaScript %}
var a;
... // 做了些甚麼

if (a) { // 被布林型轉
    console.log('a 非空值或未定義！');
}
{% endcodeblock %}

### 當 falsy 值有可能是零
由於 0 的布林型轉也會是 false，可能會造成 if 陳述句的條件判斷下剔除的對象，這時候可以搭配前篇提到的優先性（Precedence），使用優先性較高的三等號（===）加上 OR（||）來介入型轉判斷：
{% img /images/js-weird-parts-boolean-vs-existence/0.png 800 200 js-weird-parts-boolean-vs-existence %}
{% codeblock lang:JavaScript %}
var a;
if (a || a === 0) {
    console.log('0會被列入考慮');
}
{% endcodeblock %}
由於優先性先判斷後方 `a === 0` 為 true，繼續判斷表達式`a || true` 為 `false || true`，OR（||）兩邊任一方為 true 印出 true，條件式成立印出 console.log('0會被列入考慮')。