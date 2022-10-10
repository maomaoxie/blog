---
title: 克服 Javascript 的奇怪部分 運算子
date: 2022-09-21 14:28:51
tags:
categories:
---

在閱讀本篇文章以前，先給予一個結論方便後續理解：
運算子不過就是一種函式而已，而且會回傳一個運算結果的值．

Javascript 的運算子（operators）有個奇妙的現象就是，運算過程會造成動態型別的轉換而獲得不如預期的結果．

# 運算子（operators）的定義
### 實際上為一種函式
{% blockquote %}
A special function that is syntactically (written) differently.
{% endblockquote %}

### 需要兩個參數獲得結果
{% blockquote %}
Generally, an operator take two parameters and return one result.
{% endblockquote %}

### 中綴寫法
運算子其實就是一種表達式必備的元素，丟進去參數然後運算 return 結果，只是不同於一般的函式寫法，它是使用中綴（infix　notation）的寫法，將參數寫在函式符號的兩邊：
{% codeblock lang:JavaScript %}
var a = 3 + 4;
console.log(a); // 7
{% endcodeblock %}

老式的計算機都是使用後綴的寫法：
{% codeblock lang:JavaScript %}
3 4 +;
{% endcodeblock %}

### 型別轉換
Javascript 的運算子造成的隱式型別轉換數不勝數，例如比較運算子（Relational operators），會將數字型別轉換成布林值（Boolean）。
{% codeblock lang:JavaScript %}
var a = 3 > 4;
console.log(a); // false
{% endcodeblock %}

# 結論
{% blockquote %}
Operators are actually spacial types of functions, these parameters are being pass into those functions,
and the value  is being returned.
There is pre written codes that javaScript engine provides to do or invoke those functions.
{% endblockquote %}
運算子只是已經寫好的函式（function）提供使用而已，寫法稍有不同於一般函式，為中綴（infix　notation）的寫法。