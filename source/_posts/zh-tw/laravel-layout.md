---
title: Laravel 善用 layout 避免重複的模板
date: 2022-04-10 14:54:20
tags:
- laravel
- blade
- layout
categories: laravel
---
{% img /images/laravel-layout/0.png 800 200 laravel-layout %}

在設計 UI 模板時重複使用的內容屢見不鮮，例如 header、footer 或是 sidebar 等多是網站中分頁裡的標配，這時候重複貼上一樣的模板就顯得有點呆，Laravel MVC 架構中 views 可以創建一個 layout 模板資料夾，重複的 UI 配置可以放置在這裡提供其他分頁的 blade 檔案使用，這樣一來也能省去 layout 修改時相關的檔案都要更改的麻煩事，統一在 layout 裡調整即可：

# 建立 layout 資料夾
此資料夾可以存放共用的模板，例如 header、footer 或是 sidebar 的模板 html。
在 project 裡的 `resources/views` 建立一個 layout 資料夾：
{% img /images/laravel-layout/1.png 300 200 laravel-layout %}
______

# 撰寫模板
確立好 controller 中與 views 與 routes 的連結後，就可以著手進行模板的拆分啦！

### @extends('layoutFolder/layout')
承接共用模板的其他 views 則使用此語法接收模板，撰寫資料夾路徑方式有兩種：
1. 斜線
2. 點

{% codeblock about.blade.php lang:JavaScript %}
@extends('layout.layout')
{% endcodeblock %}
{% codeblock about.blade.php lang:JavaScript %}
@extends('layout/layout')
{% endcodeblock %}

### @yield('content')
yield 像是在告訴 php 說我這裡需要挖個洞，待會會丟 html 進來，請認名稱來辨識丟進來的檔案：
{% codeblock layout/layout.blade.php lang:JavaScript %}
  很多要共用的 html
  ...(通常是 header)
  @yield('content')
  很多要共用的 html
  ...(通常是 footer)
{% endcodeblock %}

### @section('content') ... @endsection
section 類似 vue 的 slot，把其他網頁要替換的內容插入這個插槽中，在通過剛才的 `@yield('content')` 將page愈替換的內容塞進去共用的模板中：
{% codeblock page.blade.php lang:JavaScript %}
  @extends('layout/layout')
  @section('content')
    這裡是 page content
    ...
  @endsection
{% endcodeblock %}
{% img /images/laravel-layout/3.png 800 200 laravel-layout %}
{% img /images/laravel-layout/2.png 800 200 laravel-layout %}
______

# 獨立每個頁面的 script
插槽的概念同樣可以挖洞給每個獨立頁面放置專屬的 javascript，在 layout 的 </body> 之前放置 @yield('after_js') 來承接 page 各自的 `<script>...</script>`：
{% codeblock layout/layout.blade.php lang:JavaScript %}
  <html>
    ...
    <body>
    ...
    @yield('after_js')
    </body>
  </html>
{% endcodeblock %}
{% codeblock page.blade.php lang:JavaScript %}
  @section('content')
    <script>
      alert("Hey!It's about page.");
    </script>
  @endsection
{% endcodeblock %}
{% img /images/laravel-layout/4.png 800 200 laravel-layout %}
______

# 簡化 @section
每個頁面會有不同的 SEO 內容，例如 `title` 或是其他的 meta data，這時候 `@section` 又派上用場了，不同的是**第二個參數**可以傳入文字檔來渲染內容：
{% codeblock layout/layout.blade.php lang:JavaScript %}
  <html>
    <head>
      <title>@yield('title')</title>
      ...
    </head>
    ...
  </html>
{% endcodeblock %}
{% codeblock page.blade.php lang:JavaScript %}
  @section('title', 'pageTitle')
{% endcodeblock %}
{% img /images/laravel-layout/5.png 300 200 laravel-layout %}
{% img /images/laravel-layout/6.png 300 200 laravel-layout %}

______
{% blockquote %}
參考資料：
https://www.youtube.com/watch?v=AGE3wRKljkw&t=2402s
{% endblockquote %}