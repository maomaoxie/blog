---
title: 克服 Javascript 的奇怪部分 單執行緒與同步執行
date: 2022-06-05 16:31:07
tags:
- Javascript
categories: javascript
---

{% img /images/js-weird-parts-single-threaded/0.png 800 200 js-weird-parts-single-threaded %}
這一個章節要來講解 JavaScript 的幾個觀念。

# 單執行緒（Single Threaded）
**一次一件事**是重點。
這個特性不是瀏覽器的特性，瀏覽器一次可能同時處理多件事情，JavaScript 引擎則是單執行緒的，就像排隊買早餐，老闆娘一次只能處理一位客人，而 JavaScript 也是一次只處理一項指令。


# 同步執行（Synchronous）
**順序**是重點。
事情有先後順序，按照順序執行，一次執行一行（或者說一個單元的程式碼，可能是一個陳述式或表達式）。

# 呼叫函式（Function Invocation）
觸發或執行一個函式，使用的符號為大括號（parenthesis）。
JavaScript 引擎在執行函式呼叫時，會發生幾件事情延續前面的章節：
{% codeblock 呼叫函式 lang:JavaScript %}
  function b () {
  }
  function a () {
    b();
  }
  a();
{% endcodeblock %}

## 創造階段
1. 全域執行環境（Global Execution Context）首先被創造。
2. 全域物件（Global object）被創造。
3. 全域 this 被創造。
4. 開始編譯階段（Parsing），編譯器巡過一遍所有程式碼發現了函式 b 與 a，在記憶體創造兩個函式的空間並且存放整個函式內容。

{% img /images/js-weird-parts-III/1.png 400 200 js-weird-parts-III %}

## 執行階段
1. 整個程式碼的記憶體準備完畢後，開始執行程式。
2. 編譯器解析到**函式 a**被呼叫，立即於全域執行環境上方，產生並堆疊一個函式 a 的執行環境（Execution Context），放進**執行佇列堆（Execution Stack）**中，每個執行環境都有自己得記憶體空間存放著變數或函式。
{% img /images/js-weird-parts-III/2.png 400 200 js-weird-parts-III %}

3. 最上方的執行佇列會優先執行，進入函式 a 的執行環境（Execution Context）並且解析到函式 b，程序暫停，立即於函式 a 的執行環境上方，產生並堆疊一個函式 b 的執行環境（Execution Context），放進**執行佇列堆（Execution Stack）**中。
{% img /images/js-weird-parts-III/3.png 400 200 js-weird-parts-III %}

以上執行階段也可以拆分成好幾個創造（執行環境），與執行（執行佇列堆最上方的執行環境）階段，在當下的執行環境執行過程中，只要觸發另一個函式，執行暫停然後創造（執行環境）、與執行（執行佇列堆最上方的執行環境），而下方的程式碼不會被解析，除非該執行環境執行完畢並且離開執行佇列堆（Execution Stack）後才會繼續逐行執行。

{% colorquote Info %}
  Everytime a function is called, a new execution context is created for that function, the `this` variable is created for that function, the variables in it were set up in the creation phase, then the code is executed line by line.
  whatever is on the top of the execution stack, is currently running synchronously.
{% endcolorquote %}

重點整理：
當一個函式被觸發或是呼叫，JS 引擎會創造一個屬於該函式的執行環境（execution context）並且放置在執行緒的最上方等待被執行，而該堆疊中具有該函式獨有的執行環境與 This，開始執行並且完成後離開堆疊中（pop out）繼續執行下一個堆疊，只要解析到新的函式被呼叫就會反覆以上行為，直到堆疊不斷（pop out）剩下全域執行環境本身為止。