---
title: 克服 Javascript 的奇怪部分 型別
date: 2022-07-30 16:35:45
tags:
- javascript
- type
categories:
---

{% img /images/js-weird-parts-dynamic-typing/0.png dynamic-typing %}
# Dynamic Typing
Javascript 的型別為動態型別（Dynamic Typing），不同於 C# 等強型別語言的靜態型別（Static Typing），無須指派型別而是在引擎執行的階段（at runtime）辨認變數記憶體內的型別為何：

{% colorquote info %}
Dynamically-typed languages are those (like JavaScript) where the interpreter assigns variables a type at runtime based on the variable's value at the time.
{% endcolorquote %}

這有可能導致一個變數在每次程式執行的結果都產生不同的型別（例如 == 型別隱式轉換），而造成不如預期的結果，所以使用三等號是比較良好的撰寫習慣。