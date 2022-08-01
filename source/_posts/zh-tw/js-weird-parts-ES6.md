---
title: 克服 Javascript 的奇怪部分 ES6
date: 2022-07-24 16:30:58
tags:
categories:
---

{% img /images/js-weird-parts-let/0.png 800 200 ES6-let %}
# ES6 let
不同於 var 的宣告方式，let 宣告的變數在宣告時會出現暫時性死區不可取用，沒有 hoisting 現象，且變數的作用域只存在於 block 區塊中，例如 if 陳述句，而 var 則是函式作用域，若撰寫在 if 陳述句內外部仍可取用。
最經典的應用就是 for 迴圈，可以在每一次的 console 正確印出數值：
{% codeblock for loop lang:JavaScript %}
for(let i=0; i<5; i++) {
  setTimeout(()=>{
    console.log(i)
  },1000)
}
{% endcodeblock %}
