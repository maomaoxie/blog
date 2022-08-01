---
title: 預處理器新朋友-Less
date: 2022-02-11 19:43:57
tags: 
- css
- less
- processors
categories: css
---

{% img /images/less-css/0.png 800 200 less-css %}
圖片來源:https://www.oxxostudio.tw/img/articles/201601/css-less-01.jpg

在接觸了新的 Vue3 + Typescript + ant design vue 專案之後，認識了新的預處理器 Less，之前只有聽過 Scss 與 Sass，來認識一下甚麼是 Less 吧！

# 為什麼要使用預處理器
預處理器存在的目的方便開發者在撰寫 css 上可以更得心應手，使用類似程式語言開發的撰寫習慣來設計網頁畫面，例如變數、迴圈甚至是嵌套樣式讓寫法更多元與方便性，可以轉換成瀏覽器可閱讀的 css 樣貌，減少在刻畫面時只能瑣碎的處理細節與重複性很高的樣式名稱，造成維護的不友善。

# Less v.s Sass ?
現今常用的幾套預處理器如下：
- SASS（SCSS）
- LESS
- Stylus
- Turbine
- Switch CSS
- CSS Cacheer
- DT CSS

Less 與 Sass 撰寫風格上很接近，以下舉例一些不同點：

### 變數的寫法
- Sass 以錢字號 $ 做開頭
  {% codeblock lang:css %}
    $mainColor: white;
    $siteWidth: 1024px;

    body {
      color: $mainColor;
      max-width: $siteWidth;
    }
  {% endcodeblock %}
- Less 以小老鼠 @ 做開頭
  {% codeblock lang:css %}
    @mainColor: white;
    @siteWidth: 1024px;

    body {
      color: @mainColor;
      max-width: @siteWidth;
    }
  {% endcodeblock %}

### 嵌套的寫法
- Sass 與 Less 一樣
  {% codeblock lang:css %}
    nav {
      a {
        color: red;

        header & {
          color:green;
        }
      }  
    }
  {% endcodeblock %}

### 繼承（延伸）的寫法
- Sass 使用關鍵字 @extend，以某個樣式當作基底延伸
  {% codeblock lang:css %}
    .error { 
      border: 1px solid red; 
      background-color: #fdd; 
    } 

    .seriousError { 
      @extend .error; 
      border-width: 3px; 
    }
  {% endcodeblock %}
- Less 使用關鍵字 :extend
  {% codeblock lang:css %}
    .a1:extend(.b) {
      color:#f00;
    }
    .a2:extend(.b all) {
    }
    .b {
      border:1px solid;
      font-size:20px;
    }
    .b.c {
      text-align:20px;
    }
  {% endcodeblock %}

### 混和的寫法
- Sass 使用關鍵字 @mixin，與繼承很像，但寫法類似獨立的模塊（module），@include 提供其他樣式引入
  {% codeblock lang:css %}
    .error { 
      border: 1px solid red; 
      background-color: #fdd; 
    } 

    .seriousError { 
      @extend .error; 
      border-width: 3px; 
    }
  {% endcodeblock %}
- Less 的混合與一般 class 樣式很像，以一個名稱後方的大括號承接外部樣式傳入的參數（parameters），編譯之後是看不到的
  {% codeblock lang:css %}
    .fn1(@v) {
      border-width: @v;
    }
    .box1 {
      .fn1(10px);
    }
  {% endcodeblock %}

參考文章：
{% blockquote %}
https://www.astralweb.com.tw/introduction-to-css-preprocessor-sass-and-less/
{% endblockquote %}