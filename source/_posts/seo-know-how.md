---
title: seo-know-how
date: 2022-08-20 10:26:25
tags:
categories:
---
SEO 分為網站結構 & 內容
網站競品 新聞類型 SEO 結構不能輸別人

搜尋引擎

# 網站連結
### 任何頁面必須可以內連
孤兒頁面對收錄到 SEO 非常扣分

#### 蜘蛛網結構
網站內部蜘蛛網結構的互連非常重要，越多就越能被 Google 搜尋到，會比提交來的重要

### 權重高的網站外連
由權重高的網站，例如新聞網站有連結過來就能大大提高收錄機會

# 網站結構
### sitemap
加入檢索隊列(Crawl Queue)的行為可以透過提交網站結構的 sitemap 至 google search console，而且不能造假也沒有無效的網頁連結（ex. 404、500 等）
sitemap是有上限的

### robots.txt 
開發階段不想曝光的網頁可以設定 robots.txt 不被收錄，但並非百分百有效
最好是設定密碼登入去擋內容，或是不要上線

# domain knowledge
領域專長
產業的核心知識 了解TA需求

# breadcrumb
網站麵包屑

# you may like
你可能會喜歡的相關文章

文章序號丟API 運算連結之間的關係(AI 資料工程師)

# js 做的連結不會收錄
可以追蹤：
{% codeblock %}
  <a href="https://example.com">
  <a href="/relative/path/file">
{% endcodeblock %}

無法追蹤：
{% codeblock %}
<a routerLink="some/path">
<span href="https://example.com">
<a onclick="goto('https://example.com')">
{% endcodeblock %}


# 網站更新頻率要勤
經常更新的網站才會被 google 將排名提前

# UI & UX 要好
網站體驗好 SEO 就會好，SEO 好廣告投入就可以降低

# 更新時間
網站的 sitemap 會註明網站更新時間，對 google 的 SEO 排名也有影響

# 網站的 canonical 標準網址
主網址會被 google 拿來計算收錄的頁面數量
{% codeblock %}
<link rel="canonical" href="" >
{% endcodeblock %}

### 網頁未加入索引
不同網址但內容一樣會扣分，記得設定 canonical

# 基本要設定的
<title>網頁標題</title>
<meta name="description" content="">
// 設定網站縮圖
<link rel="image_src" href=""/>

可以設定的
{% codeblock %}
<meta content="" name="copyright">
<meta content="" name="author">
{% endcodeblock %}

要小心的
{% codeblock %}
<meta name="keywords" content="關鍵字1, 關鍵字2" />
{% endcodeblock %}

# 社群媒體
可以透過[metatags](https://metatags.io/)協助產生網站的縮圖

# 移除外部連結的權重
其目的在於告訴搜尋引擎不要索引抓取這個連結，同時也不要給予他權重。
Nofollow 反向連結是只將 `<a href="https://example.com.tw">Example</a>` 加入 `rel="nofollow"` 標籤
<a href="https://example.com.tw" ref="nofollow">Example</a>。

# 使用者體驗
內容要與主題相符

# https
cloudflare 有提供免費的 https 

# AWD AMP PWA
提升行動裝置的網站架構亦可以提升網站體驗與速度

# 網站核心體驗三大指標
### LCP
最大內容繪製，2.5 秒可以渲染完畢

### FID
首次輸入的延遲時間，頁面互動的反應時間
### CLS
累積版面配置轉移，還未讀取到的資料先將空間撐開（lazyload），避免使用者捲到的位置空間還未撐開，例如以下：
{% codeblock %}
<img width="200" height="300" />
{% endcodeblock %}

# SEO v.s 廣告
在網站內放置廣告版位會對 SEO 扣分，但收入又需要靠廣告，是一種互相牴觸的生態

# title
網頁的 title 都要放置網站名稱，例如：
網頁的內容概要、網站名稱

# 熱門關鍵字
倘若 SEO 好熱門關鍵字也可以提升排名

# SVG 圖片格式
img 有 alt 可以幫助搜尋引擎了解圖片內容，SVG 則可以加上 title 以及 describe 等同於 img 的 alt，結構如下：
{% codeblock %}
<svg height="100" width="100" aria-labelledby="svgTitle svgDesc" role="img">
  <title id="svgTitle">Circle</title>
  <desc id="svgDesc">This is a red circle</desc>
  <circle cx="50" cy="50" r="40" stroke="black" stroke-width="3" fill="red" />
</svg> 
{% endcodeblock %}

# SERP 結構化資料
[參考資料](https://developers.google.com/search/docs/advanced/structured-data/intro-structured-data?hl=zh-tw)

### JSON-LD
結構化資料的格式，例如 Google 提供的範例：
{% codeblock %}
<html>
  <head>
    <title>Apple Pie by Grandma</title>
    <script type="application/ld+json">
    {
      "@context": "https://schema.org/",
      "@type": "Recipe",
      "name": "Apple Pie by Grandma",
      "author": "Elaine Smith",
      "image": "http://images.edge-generalmills.com/56459281-6fe6-4d9d-984f-385c9488d824.jpg",
      "description": "A classic apple pie.",
      "aggregateRating": {
        "@type": "AggregateRating",
        "ratingValue": "4.8",
        "reviewCount": "7462",
        "bestRating": "5",
        "worstRating": "1"
      },
      "prepTime": "PT30M",
      "totalTime": "PT1H30M",
      "recipeYield": "8",
      "nutrition": {
        "@type": "NutritionInformation",
        "calories": "512 calories"
      },
      "recipeIngredient": [
        "1 box refrigerated pie crusts, softened as directed on box",
        "6 cups thinly sliced, peeled apples (6 medium)"
      ]
    }
    </script>
  </head>
  <body>
  </body>
</html>
{% endcodeblock %}

顯示的內容：
{% img /images/seo-know-how/3.png 800 200 seo-know-how %}

驗證是否有誤：
[驗證連結](https://search.google.com/test/rich-results)

{% blockquote %}
參考網站：
[紅色死神 DETOOLS](https://tools.wingzero.tw/article/sn/1187)
{% endblockquote %}