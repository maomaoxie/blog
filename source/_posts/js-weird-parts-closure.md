---
title: 克服 Javascript 的奇怪部分 IIFE 與 閉包
date: 2022-11-08 11:06:33
tags:
- javascript
- closure
- IIFE
categories:
---

# 範圍練（scope chain）
範圍練（scope chain）是立即執行函式（Immediately Invoked Function Expression）與閉包的共同重要觀念，
Javascript function 的靜態物理坐落位置（physical position）深深影響著外部環境（outer environments），
也取決了那些變數是可以取用（accessible）與共享的。

經典的面試題目 for 迴圈內的變數 i 因為 var 宣告被提升（hoisting）到作用域頂層，
在調用 console.log 的時機點之前就已經不斷被覆蓋，無法呈現預期的結果：
{% codeblock for loop lang:JavaScript %}
    function buildFn () {
    var arr = [];
    for (var i = 0; i<3; i++) {
        arr.push(function () {
            console.log(i)
        })
    }
    return arr;
    // loop 執行完畢 i 記憶體儲存的是 3
    // var 會共享同一個記憶體然後不斷覆蓋
    }

    var fns = buildFn ();
    fns[0](); // 3
    fns[1](); // 3
    fns[2](); // 3
{% endcodeblock %}

為了解決記憶體內的值只是不斷被同一個指向覆蓋的問題，勢必需要獨立出每個 i 的值，有兩個做法：
1. 使用 ES6 Let 更改函式作用域（function scope）為區域作用域（block scope）
2. 使用 IIFE 的閉包特性，以下詳述

# 閉包（closure）
作用域內的變數可以為閉包所封鎖，結合 IIFE 在創造的當下就呼叫，每跑一次迴圈就產生一個獨立的作用域與執行環境（execution context），
將當前的 i 值封鎖住，可以設想為一個迴圈一個 i，並運用閉包的範圍練特性於呼叫的時候查找到正確的記憶體位址：
{% codeblock IIFE lang:JavaScript %}
    function buildFnIIFE () {
        var arr = [];
        for (var i = 0; i<3; i++) {
            let j = i;
            arr.push(
                (function (j) {
                    // scope chain 查找到閉包立馬執行時封鎖的 i 值
                    return function () {
                        console.log(j);
                    }
                }(i))
            )
        }
        return arr; // loop 執行完畢 i 記憶體儲存的是 3
    }

    var fnsIIFE = buildFnIIFE ();
    fnsIIFE[0](); // 0
    fnsIIFE[1](); // 1
    fnsIIFE[2](); // 2
{% endcodeblock %}

# 變數共享
閉包很常搭配工廠函式使用，藉助於閉包的變數共享特色，讓範圍練可以查找到儲存在記憶體位置的值，
即便外層的函式已經執行完畢並且離開執行序，仍會遺留變數的記憶體位置提供子函式使用：
{% codeblock closure lang:JavaScript %}
    function createGal (name) {
        var toyBox = [];
        return function (...toy) {
            toyBox.push(toy);
            const toys = [...toyBox];
            console.log(`女孩${name}喜歡${toys}`);
        }
    }

    var buyToyToGalMary = createGal ('Mary');
    buyToyToGalMary('玩具熊');
    buyToyToGalMary('洋娃娃');
    buyToyToGalMary('吹泡泡');

    var buyToyToGalAda = createGal ('Ada');
    buyToyToGalAda('小火車');
    buyToyToGalAda('佩佩豬');
{% endcodeblock %}
