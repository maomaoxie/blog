---
title: 克服 Javascript 的奇怪部分 Hoisting
date: 2022-06-04 16:28:20
tags:
- javascript
categories:
---
{% img /images/js-weird-parts-II/0.png 800 200 js-weird-parts-II %}
課程作者提到，**Hoisting** 大約是 JavaScript 裡最奇怪的行為了，而且是其他語言無法做到的特性，來了解一下怎麼回事吧！

# Hoisting
這一段程式碼若在其他語言執行可是會發生錯誤的！但是在 JavaScript 裡卻可以安然無恙：
{% codeblock lang:JavaScript %}
  b();
  console.log(a);
  var a = '123';
  function b () {
    console.log('b is called!');
  }
{% endcodeblock %}
{% img /images/js-weird-parts-II/1.png 800 200 js-weird-parts-II %}
a 竟然在還未被宣告以前使用，會回傳 undefined；b 函式正常運作。
這就是 JavaScript 的 Hoisting 在搞鬼，但不要被 Hoisting 的提升之意混淆了，該現象並非程式碼被靜態的（physically）提升到最上方，我們來解析一下編譯器是如何執行這一段程式碼：

# 執行環境執行的兩階段
{% blockquote %}
There are two phases when it came to the execution context within the Javacript engine:
The first phase was the creation phase, when it sets up the variables and functions in memory. And the second phase was the execution phase, all those things already being set up, so now it runs your code line by line.
{% endblockquote %}

## 創造階段
在第一個章節有提到，Javascript 並非是完全直譯的語言，其中一個佐證就是 Hoisting 的行為，如果程式碼真的是逐行翻譯然後執行，它是怎麼知道變數 a 與函式 b 會被創建的呢？

這裡就能理解編譯器在執行程式以前仍存在一段**編譯完成才執行**的過程：
Javascript 引擎先將整個程式碼審視一遍，找出所有具有名稱（variable name）、**並非**透過區塊作用域（Block scope）的關鍵字 const 與 let ，而是 var 所宣告的的值或函式找出來，然後歸納：誰是變數就給予一個記憶體空間，存放著未定義的值（undefined）；誰是函式就給予一個記憶體空間，存放著整個函式的內容。
{% img /images/js-weird-parts-II/2.png 800 200 js-weird-parts-II %}

也就是說創造階段可以這樣理解：
{% codeblock lang:JavaScript %}
  var a;
  function b () {
    console.log('b is called!');
  }
{% endcodeblock %}

變數 a 與函式 b 都各自被建立一個記憶體空間，接著存放著對應的值，變數 a 的記憶體空間內存放了 `undefined`，函式 b 的記憶體空間則直接放置整個函式內容。
關鍵就在變數的創造階段，只是**宣告（declared）**而已，這個階段 JavaScript 引擎並不清楚變數 a 將來的值會是甚麼，直到執行階段才會被**指派（assigned）**。

## undefined 不等於 not defined
當變數通過 var 關鍵字宣告，記憶體就被建立並且放入 `undefined` 的值，如果變數未宣告就使用，則瀏覽器會噴錯誤訊息：
{% codeblock lang:JavaScript %}
console.log(a);
// Uncaught ReferenceError: a is not defined
{% endcodeblock %}
這是瀏覽器表示：嘿！我在任何記憶體都找不到這個名稱的參照，甚至連 `undefined` 都不是。

### 小知識
{% colorquote info %}
Since undefined is a longer string than null, the JIT compiler has to save 4 bytes more to memory when using undefined instead of null while parsing. Consider that memory aswell.
{% endcolorquote %}
`undefined` 並非不存在的值，它是 Javacript 中的原始型別，也是純值的一種，甚至占了記憶體 4 個 bytes ，比空值`null`還要多。

## 良好的 coding 習慣

#### (X)在宣告以前使用變數
為了避免在執行過程被 Hoisting 汙染，最好養成先宣告後調用或賦值的習慣，可以避免一些錯誤發生，這也是為什麼 Eslint 或 Airbnb 等大宗規範都建議的撰寫規則。

#### (X)將變數賦值為 undefined
{% codeblock lang:JavaScript %}
a = undefined;
{% endcodeblock %}
倘若你這麼做了，會在除錯的時候難以辨認是 Javacript 引擎設定的還是後來你撰寫的程式所賦值的。

# 結論
若要將 Hoisting 給予一個較好的解釋，我想就是「創建初始化」，給予一個初始的值以便後續利用：函式就直接賦值，變數的則填補上執行等號（=）以後的值，不過那已經是下一個**執行階段**的任務了。
