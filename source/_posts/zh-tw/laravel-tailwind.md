---
title: 在 Laravel 專案導入 tailwind css
date: 2022-04-24 16:47:56
tags:
- tailwind
- laravel
categories:
- tailwind
---
{% img /images/laravel-tailwind/0.jpg 800 200 在 Laravel 專案導入 tailwind css %}
導入 tailwind css 之前有幾個與 Laravel 執行環境有關的雷需要注意：

# 前情提要
將 Laravel 專案放置在 `\\wsl$` 路徑內可以更有效率的執行 WSL2，跑起來的速度也快，不過要注意專案環境包相關聯的耦合性，例如 Docker 設定、php 版本對於某些指令的支援度。
{% colorquote info %}
以下網址介紹了 WSL2 建置的專案具體位置究竟在哪：
https://solidstudio.io/blog/windows-subsystem-for-linux-explained
{% endcolorquote %}
由於在 windows 系統中建置 Laravel 需要 Linux 子系統 WSL2 的支持，而使用 artisan 相關的指令都建是使用 Windows Terminal 下的 ubuntu 終端機會更友善些：
{% colorquote info %}
 There is however one great application that makes running the WSL console easier. It’s Windows Terminal and it can be installed from Windows Store. It automatically detects any WSL distributions installed and adds an option to run its console.
{% endcolorquote %}
至於為什麼要使用 Windows Terminal? 以上說明了**自動偵測 WSL分佈**的功能，當你打開終端機會發現他自動偵測到你使用 WSL 建置該專案包的路徑，甚至在終端機輸入 wsl 就能直接切換該系統環境。

### 系統包補丁
由於每個終端機的環境檔案包是沒有共享的，你在 powershell 很開心的安裝了一包 php，之後當你切換到 ubuntu 會發現，php 指令居然找不到?? 因為你得重裝，導入 tailwind 至 WSL 專案需要補上：

#### node.js
安裝 node 在 ubuntu的步驟
1. 新增Node.js PPA
{% colorquote info %}
PPA 有點像是系統軟體包的 package.json，記錄著軟體及其版本信息，當你運行 sudo apt update 命令時 apt 工具會根據 /etc/apt 目錄中的 sources.list 文件來決定版本是否升級。
{% endcolorquote %}
{% codeblock 安裝 node PPA lang:JavaScript %}
  sudo apt-get install curl
{% endcodeblock %}
{% codeblock 安裝 LTS 版本 lang:JavaScript %}
  curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
{% endcodeblock %}

2. 安裝Node.js
{% codeblock 安裝 Node.js 至 Ubuntu lang:JavaScript %}
  sudo apt-get install nodejs
{% endcodeblock %}

3. 查看一下版本號確保安裝成功

# 安裝 tailwind css
至 vscode terminal 或者使用 windows terminal Ubuntu 都可以安裝 tailwind css：

### 指令
{% codeblock 安裝 tailwind lang:JavaScript %}
  npm install -D tailwindcss postcss autoprefixer
{% endcodeblock %}
{% codeblock 安裝Node.js至Ubuntu lang:JavaScript %}
  npx tailwindcss init
{% endcodeblock %}

### 修改 config 加入 plugin
{% codeblock webpack.mix.js lang:JavaScript %}
  mix.js("resources/js/app.js", "public/js")
    .postCss("resources/css/app.css", "public/css", [	
      require("tailwindcss"), // <-- 加入這行
    ]);
{% endcodeblock %}

### 導入全域 css
在 `resources/css/app.css` 檔案中加入以下：
{% codeblock resources/css/app.css lang:JavaScript %}
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
{% endcodeblock %}

### 設定 tailwind.config.js
{% codeblock resources/css/app.css lang:JavaScript %}
  module.exports = { 
    content: [	
      "./resources/**/*.blade.php",  
      "./resources/**/*.js",	
      "./resources/**/*.vue",  ],
    theme: {	
      extend: {},
    }, 
    plugins: [],
  }
{% endcodeblock %}

### npm install 將 mix module 裝上
mix 指令可以打包 tailwind css 的預處理器編譯為 css，但不會自動安裝，需要 npm install：
{% codeblock lang:JavaScript %}
  npm install
{% endcodeblock %}

### 驗證是否成功
1. 在 views/layout/layout.blade.php 檔案導入全域 css：
{% codeblock lang:JavaScript %}
  <link href="{% raw %}{{{% endraw %} asset('css/app.css') {% raw %}}}{% endraw %}" rel="stylesheet">
{% endcodeblock %}

2. 加上 tailwind css 的 class
{% codeblock resources/css/app.css lang:JavaScript %}
  @tailwind base;
  @tailwind components;
  @tailwind utilities;

  .btn-primary {
    @apply py-2 px-4 bg-blue-500 text-white font-semibold rounded-lg shadow-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-400 focus:ring-opacity-75 transition-all;
  }
{% endcodeblock %}

3. 套用在 blade 檔案中
{% codeblock welcome.blade.php lang:JavaScript %}
  @section('content')
    <div class="container mx-auto">
      <h1>Hello you!</h1>
      <div class="flex flex-wrap">
        <div class="md:w-3/4.w-full">
          <div class="w-16 md:w-32 lg:w-48 transition-all bg-fuchsia-300 hover:bg-fuchsia-600 py-5 text-pink-800">123</div>
          <button class="btn-primary">My button</button>
        </div>
        <div class="md:w-1/4.w-full">aside</div>
      </div>
    </div>
  @endsection
{% endcodeblock %}

4. mix 指令打包
{% codeblock lang:JavaScript %}
  npm run mix
{% endcodeblock %}

5. 成功導入畫面
{% img /images/laravel-tailwind/1.png 800 200 tailwind 打包成功畫面 %}
{% img /images/laravel-tailwind/2.png 800 200 tailwind 打包成功畫面 %}
