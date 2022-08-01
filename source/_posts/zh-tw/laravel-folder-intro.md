---
title: Laravel 專案包初探
date: 2022-04-03 19:01:30
tags:
- laravel
- php
- linux
- blade
- wsl2
categories:
- laravel
---
{% img Laravel-folder-intro /images/laravel-folder-intro/0.png 800 200 Laravel 專案包初探  %}
Laravel 專案包建立好之後，有以下幾個主要的資料夾，一一介紹其作用：

{% colorquote info %}
  先打預防針，其實整個 Larevel 專案包預設並沒有任何 UI 框架，此專案包內名為 bootstrap 的檔案都跟該框架無半點毛關係！不要被混淆了。

  知識小學堂：
  **bootstrap** 的意思為鞋帶，衍生的動詞涵義是**啟動機制**，符合 Laravel 框架中用來充當 **glue** 黏著任何相關檔案或 app 啟動機制的程序。
{% endcolorquote %}

# vendor
Composer 相依套件的目錄，類似 node modules。

# app
核心的 code 會放在這裡，大部分的類別（class）程式放置處，重要的 MVC 架構中的 `Controller` 類別會放在 Http/Controllers 資料夾中，以處理 View 與 Model 中間的繫結處理器。

# bootstrap
這裡的 bootstrap 並非 UI 框架，而是啟動整個 Laravel app 需要使用的元件放置處，增進性能優化的快取設定（cache）會放在這，不需要編寫這邊的檔案。

# database
Model factories 會放在這裡，亦可處理 database 的遷移。

# lang
語言的切換檔案。

# public
網站的靜態檔案如 js、css 及 images 放置處，以及網站的所有入口點（entry points）`index.php` 與設定自動加載（autoloading）的地方。

# resource
MVC 架構中的 `Views` 類別會放在這裡，通常是 `blade` 模板檔案，也存放一些初胚的、未經壓縮與處理的 js、css 檔案，在網站打包之後會被壓縮並且優化性能，需要 loader 去編譯的檔案可以放在這裡，例如 less 或 sass。

### resource/bootstrap.js
該文件夾底下的 `bootstrap.js` 檔案並非 UI 框架，而是放置 `CSRF Token` 自動夾帶在 header 的程序，並且使用 `$axios` 發送 request 出去，以**避免 XSRF 跨站偽造攻擊**之用！
{% colorquote danger %}
  警告：不想整個 app 掛掉的話請不要亂動我！
{% endcolorquote %}

# routes
路由檔案，預設會有 `web.php`, `api.php`, `console.php`, and `channels.php` 幾支提供設定，也可以使用 app 裡面的 Controller Class 來渲染路由對應的 view 檔。

### routes/web.php
提供 session state、CSRF 保護與 cookie 加密，可以放置除了 server 提供的 RESTful API 以外的網頁路由設定。

### routes/api.php
API 中介軟體的設定處，是無狀態（stateless）的，所有進入該介面的路由都需要攜帶 Token 認證且無法隨意進入。

### routes/console.php
可以將自定義的指令碼撰寫在這，例如 Artisan 的相關指令。

### routes/channels.php
定義授權請求監聽的邏輯，以註冊相應的回呼函式。

# storage
放置經過編譯的 blade php 模板，以及快取與其他框架處理過後的檔案，其中`storage/app/public`路徑放置使用者操作後產生的檔案，例如頭像圖片。

# config
整個網站應用程式的設定檔案，建議熟讀並且活用其中的多樣選擇。可以把一些全域使用的環境變數放置其中。

參考文章：
{% blockquote %}
https://laravel.com/docs/9.x/structure
{% endblockquote %}
