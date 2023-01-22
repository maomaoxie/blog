---
title: 關於 Google Analytics GA 你應該知道的事
date: 2022-10-09 14:20:19
tags:
- Google Analytics
- GA
- GA4
- Google Tag Manager
- GTM
categories:
---

Google Analytics 也就是 GA ，即將在 2023 年 7 月 1 日起不再提供**通用版本（Universal Analytics）**
的資料分析了，全面升級至 GA4 是每個網站與應用程式必須面對的問題。

而訪客區分為新訪客與舊訪客，以及舊訪客的回訪率等資料。那麼 Google 要如何去區分呢？來了解一下：

# GA 如何區分與追蹤同一瀏覽人次
### client ID
該數據是 GA 用來判定此瀏覽器使用者是否為重複瀏覽的人次，紀錄方式透過 Cookie 裡的 _ga 查看內容的話，
會有一段追蹤碼記錄此用戶的 client ID，一旦清除 GA 偵測不到就會判定為新的瀏覽使用者。
{% img /images/google-analytics-knowhow/1.png 800 200 google-analytics-knowhow %}
{% blockquote author https://medium.com/peerone-technology-%E7%9A%AE%E5%81%B6%E7%8E%A9%E4%BA%92%E5%8B%95%E7%A7%91%E6%8A%80/%E4%B8%80%E7%AA%BA-ga-pageview-%E7%9A%84%E8%BF%BD%E8%B9%A4%E5%8A%9B%E9%87%8F-ga-%E7%B3%BB%E5%88%97-3-afbb42dbb8b5 一窺 GA PageView 的追蹤力量- GA 系列 (3) %}
由於 GA 判斷新訪客、舊訪客是使用「用戶裝置瀏覽器」來做區別，因此同一個用戶跨裝置訪問網站亦會被計入成兩個用戶，為了精準了解新訪客和舊訪客，事實上引入 User ID 作為統計方式會最為準確，而這一部份我待後續文章分享。
{% endblockquote %}

GA 參數（這裡指的是事件 event 參數）旨在追蹤與分析使用者在你的網站或 app 上所有的互動行為，
提供蒐集來的、具商業行為參考價值的數據。
{% blockquote google analytics https://developers.google.com/analytics/devguides/collection/ga4/event-parameters?client_type=gtm Set up event parameters %}
Parameters provide additional information about the ways users interact with your website. 
{% endblockquote %}
以下初步揭開 GA 的基礎輪廓：

# 使用者的著陸網頁（Landing Page）
訪客來到網站的第一個頁面，是整個工作階段（Session）的起始點並且開啟一個工作階段的流程，

# 基本追蹤事件
GA 有**基本追蹤事件**提供開通帳號的使用者，一窺網站或應用程式埋好 GA 追蹤碼之後的**事件（event）**概覽數據，
這篇先來簡單介紹 GA 常見的、自動蒐集的數據：

[官方說明文件](https://support.google.com/analytics/answer/9234069#user_engagement)

### 網頁瀏覽（page_view）
在指定的時間區段內，所有使用者在網站或應用程式的網頁瀏覽（造訪）量；
其中使用者的分母根據 Client ID 計次，來得知網站有多少不重複使用者。
{% img /images/google-analytics-knowhow/2.png 600 200 google-analytics-knowhow %}


### 締結互動（user_engagement）
{% blockquote google link What is user engagement on Google Analytics? %}
The User engagement metric shows the time that your app screen was in the foreground or your web page was in focus. When your site or app is running but no page or screen is displayed, Analytics doesn't collect the metric. The metric can help you understand when users actively use your website or app.
{% endblockquote %}

不負責任翻譯：
The User engagement 計量表示當你的應用程式或網站呈現在使用者畫面前景上並且被使用者聚焦，
與網站締結了互動行為，若應用程式或網站尚未載入完成或呈現出來，GA 不會蒐集該項指標。
這個計量可以幫助你了解目前應用程式或網站花了多少時間獲得使用者的聚焦，以及即時有多少活躍的使用者。

##### 互動活躍時間（engagement_time_msec）
計算使用者在與網站或應用程式締結互動之後開始計算，在網站上活躍的時間毫秒（msec）。

### campaign（活動分類）
可以在 GA 預設的 **Click 事件**裡查閱的參數，若是透過外部廣告（例如 Google Ads）等方式進入你的網站或應用程式，
campaign　參數就會被記錄，通常會顯示（referral）或（organic），有三大分類，
有時會帶括號有時不會（不知為啥知道的人請解惑一下XD）。

##### (organic)
透過搜尋而來的自然流量（主動）。

##### (referral) 
透過外部媒體網站進來（被動）瀏覽的使用者，也就是行銷常講的外連（外部連結）。

##### (cpc)
透過廣告來的。

##### (affiliate)
透過其他合作分潤的網站轉介而來。
 
##### (none)
直接進入網站的使用者，例如在 url 輸入網址。

### medium（媒介分類）
使用者透過甚麼媒介方式進來你的網站，類別同上：

### source（媒介 origin）
透過外部廣告進來的 url，例如透過 Google 搜尋進來的就會顯示 `https://www.google.com/`；
透過外部網站進來的就會顯示外部網站的 Origin URL。
{% img /images/google-analytics-knowhow/5.png 500 200 google-analytics-knowhow %}

### 事件參數
每個事件都有詳細參數可以閱覽，有機個是預設的（照順序），實際查看發現與官方文件的說明項目有出入。

# GA 如何計算一個互動的單位
### 工作階段（Session Start）
GA 的工作階段以 30 分鐘為一個單位，若這段期間使用者並未對網站進行任何互動（例如起來去廁所），
當前的工作階段就會結束。每個單位的工作流程可以包含多個互動事件（Event）或是從事商業行為，例如社交互動（Social Interaction）、電子商戶交易行為（Transaction），直到一個單位的工作階段結束（離開網站或是閒置超過 30 分鐘）。

##### 工作階段強制切割
- 只要時間來到**隔日凌晨 00:00:00 時分**，無論當前的工作階段是否有持續進行互動，都會被強制切割成另一個新的工作階段。
- 當使用者更換了網站的來源媒介（medium 例如從廣告點進來、重新搜尋網站進入），都會被強制切割成另一個新的工作階段，無論使用者是否有停止當前的工作階段。

### 到達頁面（UA 通用版稱呼）
每一個工作階段開始的第一個頁面。

### 離開率
一個工作階段中「最後一個」瀏覽的網頁，在所有網頁瀏覽量的占比。

參考資料：
[正確理解Google Analytics「工作階段」定義、計算、重要性](https://awoo.ai/zh-hant/blog/google-analytics-session/)