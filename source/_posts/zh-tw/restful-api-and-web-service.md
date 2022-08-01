---
title: 初探 Web service
date: 2022-02-24 23:20:50
tags:
- webService
- restfulApi
categories: webService
---

{% img /images/restful-api-and-web-service/0.png 600 200 restful-api-and-web-service %}
圖片來源：hiskio 課程 API 整合實戰｜RESTful 第三方串接應用

串接 API 是前後端分離的框架興盛之後最重要的課題之一啦！但是對初探網頁程式領域的菜菜子來說這個世界太廣太深，讓人望之卻步，萬事起頭難，就先來理解一下 API 武林世界裡的的兩大流派吧！

# SOAP & XML
#### 企業類型
成立已久的正宗流派，有許多資深企業的系統愛用的 API 串接方式，例如政府機構或是建案公司等老牌廠商。
#### 資料格式
傳遞資料多為較嚴謹與複雜的 `SOAP`﹑`Xml` 格式，由於規範的細節較多而且有一定的流程要走完，容易造成程式的肥大與艱澀，當用在小型專案時往往會有殺雞焉用牛刀之感，在準備期會比較漫長且學習曲線高，但是對於大型系統的整合還是有其優點。
#### 程式取向
當初由微軟所開發，所以程式取向多限制在 IIS 微軟開發體系，例如 `.net`。
___

# RESTful API
#### 企業類型
新興產業或新創公司等現代化的企業偏愛的 API 串接方式，為求快速開發多選用輕量的 `RESTful API` 以求在競爭激烈的市場上異軍突起。
#### 資料格式
傳遞資料多為簡單易懂的 `Json`，開發程式也容易快速，可以靈活地符合大或小的資料需求．不過相對來講規範與細節也比較粗糙與鬆散，需要在準備期規範好 API 的命名規則與資料邏輯，不然容易導致 API 很多但功能性重複等問題。
#### 程式取向
任何程式語言都可以開發出來的 API．不侷限於哪種體系的程式，例如 `python`﹑`php`﹑`ruby`﹑`node.js`等等。

而今的開發環境裡新興的流派已經逐漸為 RESTful API 所佔有，資深或金融機構也逐步轉型為輕量的 API 資料交換方式。
___

# SOAP v.s REST
#### REST 優於 SOAP
{% blockquote %}
REST 提供更多元資料格式。SOAP 只有 XML。
基於 JSON，REST 被公認是易於處理的。
基於 JSON，REST 提供對瀏覽器更好的支援。
REST 提供優越的性能。特別指快取資料。
世界級大公司主要服務協定。
REST 一般而言比較快且省頻寬。
{% endblockquote %}

### SOAP 優於 REST
{% blockquote %}
SOAP 是標準 HTTP 協定，能更方便通過防火牆和Proxy而不對協定本身進行修改。
如果你需要處理 ACID 交易，那麼 SOAP 是不錯的方向。
SOAP 擁有各種 WS- 擴充服務，擁有高可擴充性。
B2B 的世界，安全與穩定重於一切。
{% endblockquote %}
___
{% blockquote %}
Bruce 的一些心得

青菜蘿蔔各有所好，並不一樣是 REST 完勝，例如，之前有一陣子在忙的 B2B Biztalk 專案，在加解密(安全)部分，基於 XML 的 SOAP 就比 REST 更為合適。但基於 XML 的協定有個很明顯的缺點，就是當來源資料量成長一些些，產出的 XML 資料量會明顯成長很多。資料解析下，拜 JSON.NET 之類的框架(如之前介紹過的 Jil)幫忙，JSON 格式的處理是非常簡單的。但如果有一天，你真的碰到要處理 XML，那麼不要忘記 LINQ to XML 這個技術就好。
{% endblockquote %}

參考資料：
>https://blog.kkbruce.net/2018/04/soap-with-rest-good-parts.html#.YheofuhBxPY