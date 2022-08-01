---
title: Laravel public 靜態檔案
date: 2022-04-10 17:11:18
tags:
- laravel
- public
- static
categories: laravel
---
{% img /images/laravel-public/0.png 800 200 Laravel public 靜態檔案 %}
雖然檔案打包是現今網頁開發的趨勢，但網站難免需要引用未經編譯的靜態檔案，以避免編譯後的亂數檔名都要經過打包程序才能使用，Laravel 專案包中的 `public` 資料夾就是靜態檔案的去處，與之相對應會被壓縮及打包的動態檔案則要放置在 `resources` 資料中：


# 靜態 css 檔案
在 public/css/style.css 存放網站的主要設計檔，並且在主模板 `resources/views/layout/layout.blade.php` 中引用該靜態檔案之路徑在 `<link>` 中：
{% codeblock layout/layout.blade.php lang:JavaScript %}
  <html>
    <link rel="stylesheet" href="css/style.css">
    ...
  </html>
{% endcodeblock %}
但問題來了！當路由來到巢狀或雙層的時候，絕對路徑的悲劇就會發生，若我們將路由改成：
{% codeblock web.php lang:php %}
  Route::get('/about/about', [homeController::class, 'about']);
{% endcodeblock %}
{% img /images/laravel-public/1.png 800 200 Laravel public 靜態檔案 %}
你會看到 css 檔案的路徑改變成 `http://localhost:3000/about/css/style.css`，因為找不到 about 底下的 css 檔案，所以整個 style 返回 404：
{% img /images/laravel-public/2.png 800 200 Laravel public 靜態檔案 %}

___

# url function
你想到的問題 Laravel 怎麼會漏掉？這時候可以使用 blade 模板引擎包裝好的 `url 方法`，傳入相對靜態檔案路徑當參數,，來抓取靜態檔案的正確路徑：
{% codeblock layout/layout.blade.php lang:php %}
<link rel="stylesheet" href="{% raw %}{{{% endraw %} url('css/style.css') {% raw %}}}{% endraw %}">
{% endcodeblock %}

運作正常：
{% img /images/laravel-public/3.png 800 200 Laravel public 靜態檔案 %}


