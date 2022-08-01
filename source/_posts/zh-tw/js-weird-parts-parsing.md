---
title: 克服 Javascript 的奇怪部分 名詞解釋篇
date: 2022-06-04 11:07:31
tags: 
- javascript
categories:
---
{% img /images/js-weird-parts-I/0.png 800 200 js-weird-parts-I %}
先前滿常看到別人推薦的 Udemy 課程：克服JS的奇怪部分，原來自己已經購買且塵封在閱讀清單已久XD
馬上來匹乓一下吧！


# 執行篇

## 語法解析器（Syntax Parser）
將你撰寫的 Javascript 編譯成電腦可以理解的一套程式語言（program），並且決定語法執行的方式以及是否合乎規範的角色，也與直譯（interprets）、轉譯（Compiler）概念雷同，其中直譯的編譯方式為逐字編譯後由電腦執行，大部分的 Javascript 程式都是直譯的，但並非全部情況。
編譯的過程會將 Javascript 拆解分類，依變數、函式等轉換成電腦硬體可以閱讀的語言，然後執行。
{% img /images/js-weird-parts-I/1.png 800 200 js-weird-parts-I %}
可以想像成生產線上的作業員，持有原料工序可以生產產品。

## 靜態詞法作用域（Lexical Environments）
{% colorquote info %}
Where something **sits physically** in the code you write,
determines how it interacts with other elements in the program.
{% endcolorquote %}
**撰寫**的時候決定如何運作。
你撰寫語法的靜態物理位置，決定了該對象（變數、函式等）與其他對象的互動與執行關係，並且被直譯器透過這個物理關係，或者說詞彙或文法關係來統整歸坐落在硬體記憶體中的位置，但須注意的是並非所有語言皆如此（java、C# 等為動態作用域）。
這有助於上方的語法解析器來決定程式怎麼運作，你撰寫 code 的位置在哪？周圍有甚麼、被甚麼包圍都很重要！
可以想像成生產線上的主管，歸納好產品的原料工序與存放倉庫的規則。

## 執行環境（Execution Context）
{% colorquote info %}
**A wrapper** to help manage the code that is running.
There are lots of lexical environments, which one is currently running is managed via execution context.
{% endcolorquote %}
**執行**的時候決定如何運作。
靜態作用域很多，但是執行的當下順序則是由執行環境決定，也可以稱為上下文，最常見的就是每個函式建立之後產生的`this` keyword，通常為 block 作用域所包覆，在呼叫的時候決定 this 對象。
而執行環境不是只有與你撰寫的程式碼相關而已，也包含其他東西，例如上述的 this 就是執行函式當下的編譯過程動態產生的，編譯器在幫你翻譯給電腦讀懂以前，加油添醋了一些程序使程式碼更完整、具有前後順序給電腦執行。
可以想像成工廠的老闆，決定好哪批產品先出後出、出到哪裡。

# 變數篇

## 一個名稱對應一個值
一個變數名可以更改很多次，但都只會包含一個值，而一個值裡面是更多的鍵值對。

## 物件
更多鍵值對（key-value pairs）的集合。
{% img /images/js-weird-parts-I/2.png 400 200 js-weird-parts-I %}

# 全域物件與全域環境篇（Global）
{% codeblock info lang:JavaScript %}
  The base execution context is your global execution context.
  Things that are accessible everywhere to everything to your code.
  And it creates 2 things that you don't have to write about:
  1. Global object
  2. this
{% endcodeblock %}

## 全域執行環境
全域執行環境是所有 Javascript code 執行的基礎，所有的執行起點都存在於該環境內（being wrapped）。並且 Javascript 引擎會在執行初始建立兩個對象：全域物件與 this，即使你沒有撰寫任何程式碼，Javascript 引擎仍會自動產生。

## this
其中全域執行環境中，瀏覽器底下的 this 就是指向 **window** (Global Object)。
weird-parts-I/3.png 600 200 js-weird-parts-I %}

## 全域物件
若 Javascript 引擎是在後端執行，例如 node.js，則 全域物件就不會是 window。
當你在全域執行環境下靜態的撰寫變數或函式，並且詞法作用域上開放而沒有撰寫在其他函式內，就會自動附著（attached to）在全物域物件底下，全域物件意味著整個 Javascript 中的任何其他詞法作用域或者任何對象乃至整個檔案，都可以取用（accessible）這些資料。
{% img /images/js-weird-parts-I/5.png 400 200 js-weird-parts-I %}
{% img /images/js-weird-parts-I/6.png 500 200 js-weird-parts-I %}

以下都是指向同一個記憶體位置、同一個值：
{% img /images/js-weird-parts-I/7.png 500 200 js-weird-parts-I %}

在執行環境中，還有一個**外部環境（Outer Environment）**沒有提到，而在全域執行環境的層級中，外部環境是 null，因為全域本身就是最外層的 wrapper，沒有更外層的環境了。
{% img /images/js-weird-parts-I/8.png 700 200 js-weird-parts-I %}

