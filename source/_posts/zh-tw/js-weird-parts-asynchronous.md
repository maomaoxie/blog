---
title: 克服 Javascript 的奇怪部分 如何執行非同步
date: 2022-07-24 16:56:25
tags: 
categories:
---
{% img /images/js-weird-parts-asynchronous/0.png 800 200 asynchronous %}

瀏覽器在運作的時候有三大功能要執行：render engine 畫面渲染 -> JavaScript 引擎 -> Http Request。

{% colorquote info %}
A browser engine (also known as a layout engine or rendering engine) is a core software component of every major web browser. The primary job of a browser engine is to **transform HTML documents and other resources of a web page into an interactive visual representation on a user's device**.
{% endcolorquote %}

雖然名詞解釋上為非同步，但在 JavaScript 運作上實際仍是 line by line，而且具有先後順序的，對瀏覽器而言以下三項機制才是同時運作（asynchronous）：

{% img /images/js-weird-parts-asynchronous/1.png 800 200 asynchronous %}

# Http request & response
客戶端發送了頁面請求後，**Http 機制**開始運行收發請求與回應網路資源的文本協定，建立瀏覽器與伺服器的溝通橋樑，作為 TCP/IP 的應用層，並且將資料回應提供給 JavaScript 來處理。

# Javascript
一旦提及非同步就不可埋沒一大功臣，JavaScript 引擎中的**事件佇列（Event Queue）**。

#### 事件佇列（Event Queue）
當 Javascript 引擎執行完執行佇列（Execution Stack）的內容後，也就是執行佇列已經清空後，會定期（periodic）來檢視事件佇列（Event Queue）的事件排序並且執行，例如 Click 事件的回呼函式，或者是 API 收發資料的任務，才會被放置到執行佇列（Execution Stack）然後執行。在 Javascript 中的運作仍是同步的，並沒有非同步在執行程式。

#### 微任務與宏任務
部分的執行任務會被放置在事件佇列中，待執行佇列（Execution Stack）所有任務完成後才會開始執行，例如 SetTimeout（宏任務） 或是 promise（微任務） 等。

# Render engine
JavaScript 的必須仰賴瀏覽器的引擎，當瀏覽器讀取一個頁面時， JavaScript 具有觸發畫面渲染的鉤子促使**渲染引擎（Render Engine）**來改變畫面。

{% colorquote danger %}
Asynchronous means that the rendering engine, the javascript engine and the HTTP requests are running asynchrounously inside the browser, what's happening just inside the javascript is synchrounous.
{% endcolorquote %}