---
title: 克服 Javascript 的奇怪部分 原始型別
date: 2022-07-30 17:08:20
tags:
- javascript
- type
categories:
---
{% img /images/js-weird-parts-value/0.png 800 200 js-primitive-type %}
只要不是物件型別的值都可以看做是原始型別，例如：

# undefined
當一個變數還未指派任何值之前，記憶體位置會被賦予一個 undefied 的值，通常是 Javascript 引擎指派的，應該避免將任何值的預設值設定為 undefined，避免與 Hoisting 行為混淆了。


# null
若需要再資料回來之前給予判斷，可以將變數設定為 null 來表示該變數還未拿到任何值，也非 Javascript 引擎指派的值。

# boolean
true 或 false 的判斷型別，值得注意的是當值存在 localstorage 或者 cookie 時應避免儲存 true 或 false，轉換過程會強制變成 string 而造成錯誤的判斷。

# number
唯一的數字型別（numeric），不同於其他語言可能具有細緻的數字型別，例如 interger 或是 demicals，Javascript 只有一個 number type，為**浮點運算（floating point number）**，這種運算法為一個有效數字加上冪數來表示，電腦本身的二進制無法實現十進制的數字精確性，會造成數字計算上浮點位數的不正確，只能計算出近似值而已。

# string
字串型別，一串使用雙引號或單引號標記起來的文字。

# symbol
ES6 引入的新原始型別，用來表示一個獨一無二的值。產生的原因來自於物件的屬性通常都是字串（property），這樣容易造就重複的屬性而衝突，新的符號型別（symbol）於是誕生，兩個 symbol 永遠不會相等，是絕對的獨一無二。ES6 允許使用表達式 (expression) 作為屬性的名稱，語法是將 expression 放在中括號 [ ] 裡面：
{% codeblock symbol lang:JavaScript %}
let s = Symbol();
let obj = {
    [s]: function() {}
};
{% endcodeblock %}