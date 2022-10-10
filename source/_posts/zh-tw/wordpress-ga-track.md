---
title: 如何在 wordpress 埋入 GA code
date: 2022-09-15 10:42:04
tags:
categories:
---

目前 Google Analytics 已經升級至 GA4，從前綴的追蹤碼 UA- 開頭轉換為 G- 開頭，令許多前端框架的套件還趕不上變化。
以下介紹如何在 wordpress 介面插入追蹤碼：

# 在 GA 建立帳號與資源
這一步公司目前已經建置完成，建置的方法須先建立網站資源（網址 url），建立完畢後可在進階選項同時安裝新舊版的 GA code．

# wordpress 安裝 GA 步驟
### 安裝外掛 Insert Headers and Footers
安裝任何外掛程式前先 google wp 外掛黑名單，查看安全並安裝後記得跑跑看網站以免中毒．

### 埋入 GA code
舊版 UA- 開頭
{% codeblock GA3 lang:JavaScript %}
    <!-- Global site tag (gtag.js) - Google Analytics -->
    <script async src="https://www.googletagmanager.com/gtag/js?id=UA-XXXXXXXXX-1"></script>
    <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());

    gtag('config', 'UA-XXXXXXXXX-1');
    </script>
{% endcodeblock %}

新版 G- 開頭
{% codeblock GA4 lang:JavaScript %}
    <!-- Google tag (gtag.js) -->
    <script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
    <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());

    gtag('config', 'G-XXXXXXXXXX');
    </script>
{% endcodeblock %}