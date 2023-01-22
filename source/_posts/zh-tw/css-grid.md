---
title: css grid 切版介紹
date: 2023-01-15 10:28:28
tags:
- css
- grid
categories: css
---

在處理特殊切版的時候，例如雜誌的多格照片排版時 grid 要靈活許多，不過 flex 亦有他的優勢在，以下這一篇介紹 grid 切版的應用上以及注意的雷點。

二維的觀念來排版
就像切 table 一樣來分配面積
flex 處理不了的通常會使用 grid
inline-grid inline-flex 的用法? 跟內容物一樣寬
容器設定為 display grid 

# 常用的 grid 屬性
___
### 分配等份
#### grid-template-columns 橫向（欄）切版
`grid-template-columns` 將裡面的元素歸納**欄方向（column）**等分去切版，可以分橫向的等分。
`grid-template-columns` 較 `grid-template-rows` 常用。

`grid-template-columns: auto auto;` 一排兩等分
`grid-template-columns: auto auto auto;` 一排三等分

#### `auto` 會拿剩下的空間平均分配
同時使用 `auto` 與 `fr` 的話，fr會拿剩下的空間平均分配；而 auto 會變成容器本身的寬度（inline-block）。

{% colorquote info %}
flex grow 未必會按照比例分配空間，需要搭配 `flex-basis` 才能達到與 fr 相同的等分比例。
{% endcolorquote %}

#### repeat 精簡等份寫法
{% codeblock grid lang:css %}
.col {
  display: grid;
  /* grid-template-column: 1fr 1fr 1fr 1fr; */
  grid-template-column: repeat(4, 1fr); // 上面的簡寫版本
}
{% endcodeblock %}

#### grid-template-rows 設定縱向（列）切版
grid-template-rows 將裡面的元素歸納**列方向（rows）**等分去切版，可以分縱向的等分。
局部等比例區塊縮放時使用，大版面較少用到

grid-template-rows: auto; 與容器一樣高
grid-template-rows: 100px; 客製高度

相對於 column 寫的單位會影響整體分配來說；rows 則是有寫的指定位置才設定大小，沒寫的預設 auto。
若沒有給予高度就是內容本身高度

___
### gap 間距
`grid-column-gap` & `grid-row-gap` 的第一個與最後一個元素只會有一邊的間距
除非 wrapper 加上一個 padding

#### grid-column-gap 設定橫向間距
欄與欄之間的間距

#### grid-row-gap 設定縱向間距
列與列之間的間距

#### grid-gap
同時設定列與欄之間的間距
{% codeblock grid-gap lang:css %}
  .item {
    grid-gap: 10px; // 統一設定
    grid-gap: 10px 20px; // 先列後欄
  }
{% endcodeblock %}
___
### justify & align 橫向或縱向對齊
grid 對齊方式屬性與 flex 類似具有 `justify-content` 橫向對齊與 `align-content` 縱向對齊。
{% colorquote info %}
須注意對齊是需要有多餘空間的條件下，
以及搭配 `grid-template-columns: repeat(?, auto);`，才可以達成對齊目的，
`grid-template-columns: repeat(?, 1fr);` 會吃掉全部的空間導致對齊無感。
{% endcolorquote %}

#### space-between 對齊左右邊 或上下邊
{% img /images/css-grid/11.png 660 200 imgTag %}

#### space-around 四周包圍的平均間距
{% img /images/css-grid/10.png 660 200 Mawchu 貓奴前端的天空 - css grid 切版介紹 %}

___
### 設定範圍
#### grid-column & grid-row
使用 template 切好格線劃分後，使用 `grid-column` 指定欄位置與 `grid-row` 指定範圍，
數字代表第幾條**格線**的位置開始放置指定的 element

##### 斜線設定範圍
- html 切版
{% codeblock grid lang:html %}
  <div class="wrapper">
    <div class="item one">one</div>
    <div class="item two">two</div>
    <div class="item three">three</div>
    <div class="item four">four</div>
  </div>
{% endcodeblock %}

- css 設定
{% codeblock grid lang:css %}
  .wrapper {
    max-width: 660px;
    height: 600px;
    background: #ccc;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(4, 1fr);
  }
  .item {
    border: 1px solid #aaa;
    padding: 10px;
  }
  .one {
    background: rgba(234, 249, 178, 0.6);
    grid-column: 4;
    grid-row: 2;
  }
  .two {
    background: rgba(249, 199, 178, 0.6);
    grid-column: 2;
    grid-row: 1/3; // 從第一列佔到第三列
  }
  .three {
    background: rgba(203, 98, 255, 0.6);
    grid-column: 1/4;
    grid-row: 2/4;
  }
  .four {
    background: rgba(98, 184, 255, 0.6);
    grid-column: 3/4;
    grid-row: 2/5
  }
{% endcodeblock %}

從圖片可以看到區塊之間是獨立的空間，可以用 photoshop 的圖層概念來思考，重疊也是沒問題的！
{% img /images/css-grid/15.png 660 200 Mawchu 貓奴前端的天空 - css grid 切版介紹 %}

#### grid-auto-rows 設定每一列的高度
grid-auto-rows 可以直接指定列的高度為多少，搭配 `minmax()` 還可以設定最大最小值：

- css 設定
{% codeblock grid lang:css %}
  .wrapper {
    max-width: 660px;
    /* height: 600px; */
    background: #ccc;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    /* grid-template-rows: repeat(4, 1fr); */　這裡要記得移除
    grid-auto-rows: minmax(80px, auto);
  }
  .item {
    border: 1px solid #aaa;
    padding: 10px;
  }
  .one {
    background: rgba(234, 249, 178, 0.6);
    grid-column: 4;
    grid-row: 2;
  }
  .two {
    background: rgba(249, 199, 178, 0.6);
    grid-column: 2;
    grid-row: 1/3;
  }
  .three {
    background: rgba(203, 98, 255, 0.6);
    grid-column: 1/4;
    grid-row: 2/4;
  }
  .four {
    background: rgba(98, 184, 255, 0.6);
    grid-column: 3/4;
    grid-row: 2/5
  }
{% endcodeblock %}

{% colorquote danger %}
使用 `grid-auto-rows` 需要移除 `grid-template-rows` 否則屬性會被覆蓋
{% endcolorquote %}

當容器本身的高度小於最小值時，該容器會撐高至最小值：
{% img /images/css-grid/16.png 660 200 Mawchu 貓奴前端的天空 - css grid 切版介紹 %}

當容器本身的高度超過最小值時，該容器會自適應自動高度：
{% img /images/css-grid/17.png 660 200 Mawchu 貓奴前端的天空 - css grid 切版介紹 %}

### grid-template-areas 命名配給範圍
`grid-column` & `grid-row` 是指定格線的範圍來配給空間給元素，`grid-area` 則是透過命名來配給空間給元素，兩者的概念類似但方法不同：

#### 搭配 `grid-template-columns` 單位 `fr` 等份切割
- html 切版
{% codeblock grid lang:html %}
  <div class="container">
    <header>header</header>
    <nav>nav</nav>
    <main>main</main>
    <aside>aside</aside>
    <footer>footer</footer>
  </div>
{% endcodeblock %}

- css 設定
{% codeblock grid lang:css %}
  .container {
    max-width: 960px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-areas: "header header header header"
    "nav nav nav nav"
    "main main main aside"
    "footer footer footer footer";
  }
  header {
    background: burlywood;
    grid-area: header;
  }
  nav {
    background: lightskyblue;
    grid-area: nav;
  }
  main {
    background: lightgreen;
    grid-area: main;
  }
  aside {
    background: lightpink;
    grid-area: aside;
  }
  footer {
    background: lightsalmon;
    grid-area: footer;
  }
{% endcodeblock %}

##### grid-area 命名元素
命名配給的對象；通常是子元件。

##### grid-template-areas 指定元素
給予空間範圍內配給的對象，按照配給等份放置名；通常是父容器。

{% blockquote 死神老師 排版精隨 link_title %}
  大範圍使用 flex，小範圍局部使用 Grid 排版。
{% endblockquote %}

#### grid 切板搭配 aspect-ratio
圖片的比例 `aspect-ratio` 可以影響切版的面積大小，例如以下的範例將畫面均分四等份後，
面積大小會按照圖片的寬高比改變：

- html 配置
{% codeblock grid lang:html %}
<div class="wrapper">
  <div class="picture-top">
    <img class="object-cover" src="./taipei-1.jpg" alt="">
  </div>
  <div class="picture-bottom-left">
    <img class="object-cover" src="./taipei-2.jpg" alt="">
  </div>
  <div class="picture-bottom-right">
    <img class="object-cover" src="./taipei-3.jpg" alt="">
  </div>
</div>
{% endcodeblock %}

- 不設定比例
{% codeblock grid & aspect-ratio lang:JavaScript %}
.wrapper {
  aspect-ratio: 1/1;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
  grid-gap: 12px;
  grid-template-areas: "top top"
  "bottom-left bottom-right";
}
.picture-top {
  grid-area: top;
}

.picture-bottom-left {
  grid-area: bottom-left;
}
.picture-bottom-right {
  grid-area: bottom-right;
}
{% endcodeblock %}
{% img /images/css-grid/18.png 460 200 Mawchu 貓奴前端的天空 - css grid 切版介紹 %}

- 設定寬高比 2:1
{% codeblock grid & aspect-ratio lang:JavaScript %}
.picture-top {
  grid-area: top;
  aspect-ratio: 2/1;
}
{% endcodeblock %}

{% img /images/css-grid/19.png 460 200 Mawchu 貓奴前端的天空 - css grid 切版介紹 %}
