---
title: Php 基礎知識
date: 2022-04-10 18:10:18
tags:
- php
categories: php
---
{% img /images/php-basic-knowledge/0.png 800 200 php 基礎知識 %}
在 javascript 裡面訪問屬性的方式很單純，都是使用 `.` 來進入傳參考的對象記憶體中存放的值、訪問類中的屬性還有創立物件內的 key 與 value，然而在 php 中則用三種不同的符號來達成不同目的：


### :: 範圍解析操作符
用來訪問類（class）底下的對象。
{% codeblock layout/layout.blade.php lang:php %}
  $classname::CONST_VALUE
{% endcodeblock %}

### -> 指向符號（瘦箭頭）
用來訪問物件內某類別的值，類似 javascript 裡頭的 `.`。 
{% codeblock layout/layout.blade.php lang:php %}
  $object->property='value'
{% endcodeblock %}

### => 指向符號（胖箭頭）
用來創建物件內的鍵值對，一個 key 對上一個 value，類似 javascript 裡頭的 `:`。 
{% codeblock layout/layout.blade.php lang:php %}
  $array = array("key" => "value");
{% endcodeblock %}