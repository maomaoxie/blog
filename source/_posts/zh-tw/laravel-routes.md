---
title: Laravel 認識路由
date: 2022-04-04 15:41:43
tags:
- laravel
- php
categories:
- laravel
---
{% img laravel-routes /images/laravel-routes/0.png 800 200 Laravel路由介紹 %}
應用程式介面的主要目的就是提供使用者與介面互動（Interaction），其中 URL 可以取得使用者的資訊並且渲染在畫面中，例如 query string，就是透過最基本的 GET request 來獲取信息：

___
# routes
在路由頁面設定想要回應到前端的資訊，有幾種方式：

### simple data
回傳簡單的關聯式陣列：
{% colorquote info %}
關聯式陣列可以想做類似 javascript 的物件一樣，鍵值對（key-value pair）的形式．
{% endcolorquote %}

{% codeblock routes/web.php lang:Php  %}
  Route::get('/greetings', function () {
    $datas = [
      [ 
        "name" => "Abby"
      ]
    ];
    return $datas;
  });
{% endcodeblock %}
{% img laravel-routes /images/laravel-routes/1.png 500 200 Laravel路由介紹  %}
___
### Views
回傳資料至 `blade` 模板，渲染 html
{% codeblock routes/web.php lang:Php  %}
  Route::get('/greetings', function () {
    $datas = [
      [ 
        "name" => "Abby"
      ]
    ];
    return views('greetings', $datas);
  });
{% endcodeblock %}

直接將傳遞過來的關聯式陣列 key 傳進來當變數名
{% codeblock resource/views/greetings.blade.php lang:php  %}
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Greetings</title>
  </head>
  <body>
    <h1>Hello! {% raw %}{{{% endraw %} $name {% raw %}}}{% endraw %}</h1>
  </body>
  </html>
{% endcodeblock %}

___
### query string　參數
變數也可以使用既有的 `request()` 方法承接 URL `?` 後方的 **key** 來渲染到畫面中，讀取 GET 參數：

#### request()
{% codeblock routes/web.php lang:Php  %}
  Route::get('/cat', function () {
    // 這裡輸入 params key
    $catNumbers = request('catNumbers');
    $datas = [
      "catNumbers" => strip_tags($catNumbers)
    ];
    // return "這裡有${catNumbers}隻貓";
    return view('cat', $datas);
  });
{% endcodeblock %}

{% codeblock resource/views/cat.blade.php lang:php  %}
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cat Route</title>
  </head>
  <body>
    <h1>這裡有{% raw %}{{{% endraw %} $catNumbers {% raw %}}}{% endraw %}隻貓</h1>
  </body>
  </html>
{% endcodeblock %}
{% img laravel-routes /images/laravel-routes/3.png 500 200 Laravel路由介紹  %}

{% colorquote danger %}
  為了避免 XSS 攻擊注入惡意程式到 app 中，可以為你的 request 包覆 `strip_tags()`，動態路由則不需要。
{% endcolorquote %}

#### 設置概覽資料
若使用者沒有查詢任何關鍵字（例如衣服），可以設立一個預設顯示的資料（例如展示全部商品）：
{% codeblock routes/web.php lang:Php  %}
  Route::get('/cat', function () {
    // 這裡輸入 params key
    $catNumbers = request('catNumbers');
    // 有指定的話顯示
    if(isset($catNumbers)) {
      $datas = [
        "catNumbers" => strip_tags($catNumbers)
      ];
      return view('cat', $datas);
    } else {
      // 沒指定的話顯示
      $datas = [
        "catNumbers" => '很多'
      ];
      return view('cat', $datas);
    }
  });
{% endcodeblock %}
{% img /images/laravel-routes/4.png 500 200 laravel路由 %}

{% colorquote info %}
  `isset()` 方法類似 javascript 的 `if (variable === undefined)`，判斷某變數是否存在。  
{% endcolorquote %}

___

### dynamic routes
可以渲染使用者輸入的資料，動態路由使用 `{}` 將路由變數名稱包起，爾後經過 GET URL 的方式取得信息，路由設定需要傳入變數至回呼函式中接收：

#### 範例1. 對應的動態路由變數需要傳入回呼函式中
{% codeblock routes/web.php lang:Php  %}
  Route::get('/dog/{dogName}', function ($dogName) {
    $datas = [
      "name" => "Abby",
      "dogName" => $dogName
    ];
    return view('routesTest', $datas);
  });
{% endcodeblock %}

{% codeblock resource/views/greetings.blade.php lang:php  %}
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Greetings</title>
  </head>
  <body>
    <h1>Hello! {% raw %}{{{% endraw %} $name {% raw %}}}{% endraw %}</h1>
    <h2>A dog name {% raw %}{{{% endraw %} $dogName {% raw %}}}{% endraw %}</h2>
  </body>
  </html>
{% endcodeblock %}
{% img laravel-routes /images/laravel-routes/2.png 500 200 Laravel路由介紹  %}

#### 範例2. 傳入多個非必要的動態路由參數
{% codeblock routes/web.php lang:Php  %}
  Route::get('/shop/{category?}/{item?}', function ($a = null, $b = null) {
    if(isset($a)) {
      if(isset($b)) {
        return "你正在瀏覽商店${a}分類的${b}品項";
      }
    } 
    return '你正在瀏覽商店的所有商品';
  });
{% endcodeblock %}

{% colorquote info %}
  對應的回呼函式必須接收一樣數量的變數當參數，名稱隨意不過最好與路由同名比較直覺，且位置要按照順序！
  非必要的路由參數需要設置：
  1. `?`放置在`{}`內，表示為可選的。
  2. 非必要路由參數需設置預設值為空`null`才不會噴錯。
{% endcolorquote %}

{% img laravel-routes /images/laravel-routes/5.png 500 200 Laravel路由介紹  %}
{% img laravel-routes /images/laravel-routes/6.png 500 200 Laravel路由介紹  %}
