---
title: AMP 加速行動裝置網頁介紹
date: 2022-12-05 09:47:06
tags:
- AMP
categories:
---

AMPs（Accelerated Mobile Pages）字面意義上理解為**加速行動裝置網頁**，
也就是在手機或移動裝置上優化性能的網頁框架，SEO 上也較大程度為 Google 所喜愛故而提升排行，
支援度也勝過 PWAs（Progressive Web Applications）：

{% blockquote %}
Support
PWAs are not supported equally on all devices, so you may find slight inconveniences when they're displayed on iOS. Also, they do not support all the hardware functionalities, such as Bluetooth, NFC, GPS, or accelerometers.

AMPs are supported by all major browsers on all devices.

[Progressive Web Apps vs Accelerated Mobile Pages: What's the Difference and Which is Best for You?](https://www.freecodecamp.org/news/pwa-vs-amp-what-is-the-difference-and-how-do-you-choose/)
{% endblockquote %}

雖然 AMPs 沒有如同 PWAs 一樣的 App icon 可以秀在手機桌面提供快捷，但就靜態文章的內容型網站帶來的性能、裝置的支援度與 SEO 友善等條件仍值得開發者們嘗試。

# Start up
在官方網站可以初探 AMP 框架的 HTML 樣貌：
The following markup is a basic AMP page.

{% codeblock AMP lang:html %}
<!doctype html>
<html amp lang="en">
  <head>
    <meta charset="utf-8">
    <script async src="https://cdn.ampproject.org/v0.js"></script>
    <title>Hello, AMPs</title>
    <link rel="canonical" href="https://amp.dev/documentation/guides-and-tutorials/start/create/basic_markup/">
    <meta name="viewport" content="width=device-width,minimum-scale=1,initial-scale=1">
    <style amp-boilerplate>body{-webkit-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-moz-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-ms-animation:-amp-start 8s steps(1,end) 0s 1 normal both;animation:-amp-start 8s steps(1,end) 0s 1 normal both}@-webkit-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-moz-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-ms-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-o-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}</style><noscript><style amp-boilerplate>body{-webkit-animation:none;-moz-animation:none;-ms-animation:none;animation:none}</style></noscript>
  </head>
  <body>
    <h1 id="hello">Hello AMPHTML World!</h1>
  </body>
</html>
{% endcodeblock %}

## 框架必備
AMP 對於基礎的 SEO 有嚴格要求，一旦發現缺失就無法執行應用程式，某程度的提升了 SEO 的基礎功，
強迫開發者重視並且完善之，

{% blockquote %}
AMP HTML is a subset of HTML for authoring content pages such as news articles in a way that **guarantees certain baseline performance characteristics**.
{% endblockquote %}

以下是 AMP 框架必備的 HTML 要素：

| Rule  | Description         | 
| :------------ | :-----------------------  | 
| Start with the `<!doctype html>` doctype.| Standard for HTML. |
| Contain a top-level `<html ⚡>` or `<html amp>` tag.| Identifies the page as AMP content. |
| Contain `<head>` and `<body>` tags. | While optional in HTML, this is required in AMP. |
| Contain a `<meta charset="utf-8">` tag right after the `<head>` tag. | Identifies the encoding for the page. |
| Contain a `<script async src="https://cdn.ampproject.org/v0.js"></script>` tag inside the `<head>` tag. As a best practice, you should include the script as early as possible. | Includes and loads the AMP JS library. |
| Contain a `<link rel="canonical" href="$SOME_URL">` tag inside their `<head>`. | the href attribute should point to the page itself. This section exists for legacy reasons. |
| Contain a `<meta name="viewport" content="width=device-width" />` tag inside their `<head>` tag. | Specifies a responsive viewport. |
| Contain the **AMP boilerplate** code in their `<head>` tag. | CSS boilerplate to initially hide the content until AMP JS is loaded. |

[Starter code](https://amp.dev/documentation/guides-and-tutorials/start/create/basic_markup)

## AMP 特色
### 無須額外的設定
AMP html 格式的行為與一般的無異，不須特別設定就可以為瀏覽器所理解，肇因於背後由特定的 AMP 伺服器所代理而可以衍生許多性能提升的變化與應用。
{% blockquote %}
Also, AMP HTML documents can be uploaded to a web server and served just like any other HTML document; no special configuration for the server is necessary.
However, they are also designed to be optionally served through specialized AMP serving systems that proxy AMP documents. These documents serve them from their own origin and are allowed to apply transformations to the document that provide additional performance benefits. 
{% endblockquote %}

### 集中管理客製化元素
AMP 框架提供一系列可集中管理客製化元素的高階功能，例如圖庫。
{% blockquote %}
AMP HTML uses a set of contributed but centrally managed and hosted custom elements to implement advanced functionality such as image galleries that might be found in an AMP HTML document. 
{% endblockquote %}

### 不允許透過 js 手段達成優化目的
使用 AMP 框架必須按照規格開發，不可透過自行撰寫的 JavaScript 來達成優化的目的。
{% blockquote %}
While it does allow styling the document using custom CSS, it does not allow author written JavaScript beyond what is provided through the custom elements to reach its performance goals.
{% endblockquote %}

### 友善爬蟲
使用 AMP 框架友善爬蟲快取以及第三方汲取資源。
{% blockquote %}
By using the AMP format, content producers are making the content in AMP files available to be crawled (subject to robots.txt restrictions), cached, and displayed by third parties.
{% endblockquote %}

### 優化網站效能
AMP 框架的設計目的在於縮短裝置獲取網站資源到使用者可以互動的載入時間，手法如下：

1. HTTP requests necessary to render and fully layout the document should be minimized.
2. Resources such as images or ads should only be downloaded if they are likely to be seen by the user.
3. Browsers should be able to calculate the space needed by every resource on the page without fetching that resource.

---

1. 渲染畫面必要的資源大小需壓縮。
2. 圖片或廣告進入視線範圍內才下載。
3. 瀏覽器應在獲取資源以前就計算好每個網頁資源需要的空間。

## 圖片應用
圖片與廣告在 AMP 框架內有包裝好的標籤提供使用，以提升速度與效能，撰寫方式如下：
{% codeblock AMP lang:html %}
<body>
  <h1>Sample document</h1>
  <p>
    Some text
    <amp-img src="sample.jpg" width="300" height="300"></amp-img>
  </p>
  <amp-ad
    width="300"
    height="250"
    type="a9"
    data-aax_size="300x250"
    data-aax_pubname="test123"
    data-aax_src="302"
  >
  </amp-ad>
</body>
{% endcodeblock %}