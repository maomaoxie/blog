---
title: 克服 Javascript 的奇怪部分 範圍鍊
date: 2022-07-10 15:57:42
tags:
categories:
---

{% img /images/js-weird-parts-scope-chain/0.png 800 200 scope-chain %}

以下探討的幾個議題都離不開函式（function）本身：

# 環境變數
每個執行環境（execution context）都有屬於其中的變數，可以把執行環境想作是一個**空間範圍**，而環境變數都附著在其中，例如全域變數（global variable）會附著在全域物件下，瀏覽器的全域執行環境則是屬於 window 物件，宣告在其中的變數都會隸屬於全域執行環境。

# 函式變數
函式變數在函式被呼叫並觸發後創造了一個獨特的函式執行環境，該函式內有自己的變數，此變數式在函式內**宣告（declaration）**並且創造的，只能在該函式執行環境中可以取得，稱為區域變數（scoped variable），而變數可取用的範圍稱之作用域（scope）。

# 範圍鍊
根據函式的靜態作用域、詞法作用域，也就是坐落的物理位置來向外查找可用的變數（accessible variables），而非呼叫的位置；每個函式的執行環境（execution context）都是獨立的執行堆疊（execution stack），並且都指向外部的執行環境（outer environment），一層一層的鏈結稱為範圍鍊（scope chain）。
{% img /images/js-weird-parts-scope-chain/4.png 800 200 scope-chain %}
{% colorquote info %}
值得注意的一點：函式 b 是在函式 a 呼叫並且執行之後才建立了函式 b 的執行環境。
{% endcolorquote %}