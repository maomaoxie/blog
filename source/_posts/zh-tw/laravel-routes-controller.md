---
title: Laravel 認識 controller
date: 2022-04-06 21:49:05
tags:
- laravel
- php
categories: laravel
---
{% img laravel-routes /images/laravel-routes-controller/0.png 800 200 Laravel路由控制器 %}
當路由越來越多越來越雜亂的時候，express 可以模組化路由以分類各大項目的小路由，Laravel 是基於 MVC 架構的應用程式框架，這個分類工作可以交給 **Controller** 來執行：


# init controller
使用優雅的 `artisan cmd` 就可以創建一個 Controller class 模板，在 windows powerShell 輸入`wsl` 即可切換至 Linux WSL 子系統，終端機輸入以下指令可以查詢所有 artisan cmd 的說明，記得 cd 進入專案包內才可使用 artisan 指令：
{% codeblock terminal lang:JavaScript %}
  php artisan
{% endcodeblock %}
{% img laravel-routes /images/laravel-routes-controller/1.png 800 200 Laravel路由控制器 %}

建立 Controller 模板的 artisan cmd：
{% codeblock terminal lang:JavaScript %}
  php artisan make:controller homeController
{% endcodeblock %}
{% img laravel-routes /images/laravel-routes-controller/2.png 800 200 Laravel路由控制器 %}

# 撰寫 Controller
使用**路由名稱**創建 function 並且制定渲染內容，連接 Views：
{% codeblock App/Https/Controllers/homeController.php lang:Php  %}
  <?php

  namespace App\Http\Controllers;
  use Illuminate\Http\Request;

  class homeController extends Controller
  {
    public function index () {
      return view('welcome');
    }

    // 參數要記得帶
    public function dog ($dogName) {
      $datas = [
        "name" => "Abby",
        "dogName" => $dogName
      ];
      return view('routesTest', $datas);
    }
  }
{% endcodeblock %}

{% colorquote info %}
記得將應用到的動態參數一並移植到 Controller 的 function arguments 中！
{% endcolorquote %}

# 對應 Routes 字串
將路由路徑制定好並且傳入陣列參數：
1. `use` 剛才創建的 Controller 檔案
2. 索引`[0]`放置 `ControllerName::class`
3. 索引`[1]`放置對應 `function`：
{% codeblock routes/web.php lang:Php  %}
  <?php

  use Illuminate\Support\Facades\Route;
  use App\Http\Controllers\homeController;

  Route::get('/', [homeController::class, 'index']);
  Route::get('/dog/{dogName}', [homeController::class, 'dog']);

{% endcodeblock %}

渲染結果：
{% img laravel-routes /images/laravel-routes-controller/3.png 800 200 Laravel路由控制器 %}
{% img laravel-routes /images/laravel-routes-controller/4.png 500 200 Laravel路由控制器 %}

以上就是 Controller 的常見功能。