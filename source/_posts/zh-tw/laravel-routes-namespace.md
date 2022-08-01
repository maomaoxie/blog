---
title: Laravel 命名路由
date: 2022-04-10 18:42:15
tags:
- laravel
- php
categories: laravel
---
{% img /images/laravel-routes-namespace/0.png 800 200 Laravel 命名路由 %}
當路由越來越多，也越複雜甚至有巢狀結構的時候，相對路徑就會變得複雜不直覺，這時候可以借用 laravel 的命名路由工具，以及 blade 引擎的 `route 方法` 來輕鬆渲染路由：


# 命名路由
在路由檔案 `routes/web.php` 中訪問 `name` 屬性方法並傳入制定好的路由專屬名稱：
{% codeblock web.php lang:php %}
  Route::get('/', [homeController::class, 'index'])->name('index.index');
  Route::get('/about/about', [homeController::class, 'about'])->name('index.about');
{% endcodeblock %}

___

# blade 檔案渲染路由
將制定好的路由名稱由 name 方法傳入、blade 引擎透過 route 方法接收：
{% codeblock layout/layout.blade.php lang:JavaScript %}
  ...
  <a href="{% raw %}{{{% endraw %} route('index.index') {% raw %}}}{% endraw %}" class="underline">Home</a>|
  <a href="{% raw %}{{{% endraw %} route('index.about') {% raw %}}}{% endraw %}" class="underline">About</a>
  ...
{% endcodeblock %}
{% img /images/laravel-routes-namespace/1.png 800 200 Laravel 命名路由 %}

修改路由結構測試抓取的正確性：
{% codeblock web.php lang:php %}
  Route::get('/', [homeController::class, 'index'])->name('index.index');
  Route::get('/about', [homeController::class, 'about'])->name('index.about');
{% endcodeblock %}

可以看到渲染結果如預期更新了：
{% img /images/laravel-routes-namespace/2.png 800 200 Laravel 命名路由 %}

透過 blade 引擎封裝的 `route 方法` 解決了在腦中思考相對路徑結構的煩惱，棒！