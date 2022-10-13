---
title: 克服 Javascript 的奇怪部分 預設值
date: 2022-09-26 17:15:05
tags:
categories:
---
JavaScript 引擎在函式建立當下會創造一個執行環境（execution context），以及未被建立的變數都會賦予 `undefined` 的值，相對應來講參數也是一種變數。

# 預設參數
當預設參數沒有被指定的情況下，JavaScript 引擎會默認為`undefined` 的值：
{% codeblock lang:JavaScript %}
function greetings(name) {
    console.log('Hello, ', name + '.');
}
greetings('Jamie'); // Hello, Jamie.
greetings(); Hello, undefined.
{% endcodeblock %}
這其中也有字串的隱式型轉，`Hello` + `String(undefined)`，獲得 `Hello, undefined.`的結果。

# 善用 OR（||）搭配布林型轉 設定預設值
OR（||）運算子的特性就是回傳表達式兩邊，Boolean 型轉第一個為 `true` 的值：
{% codeblock lang:JavaScript %}
function greetings(name) {
    name = name || '<你的名字>';
    console.log(name);
}
greetings(); // '<你的名字>'
{% endcodeblock %}
`name = name || '<你的名字>'` 轉換過程中會將 name 賦值 `undefined`，undefined || '<你的名字>' 透過布林型轉為 false || true，
並且返回 `true` 的值 `'<你的名字>'`，可以透過這種布林型轉小技巧來設定預設值。

# OR（||）回傳第一個 true 的值
{% codeblock lang:JavaScript %}
true || false; // true
true || true; // true
false || false; // false
undefined || '1'; // '1'
'apple' || 'banana'; // 'apple' 
{% endcodeblock %}

## 使用 OR（||）設定預設值
{% codeblock lang:JavaScript %}
undefined || 'Jakie';
null || 'existence';
'' || '預設文字';
{% endcodeblock %}