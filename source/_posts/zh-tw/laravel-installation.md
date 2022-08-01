---
title: Windows 搭建 Laravel 環境的各種跌坑
date: 2022-02-12 16:36:34
tags:
- laravel
- php
- linux
- blade
- wsl2
categories:
- laravel
---

{% img /images/laravel-installation/0.png 800 200 laravel-installation %}
使用 Window 10 搭建 Laravel 框架的過程採了一堆大坑 QQ，經過幾番努力終於成功！
千萬不要認為裝套件都是敲一敲指令就可以了，工程師的路從來都沒有那麼好走，是天堂路阿孩子！
工欲善其事，必先利其器，先將裝備整頓好才能打 BOSS：

# 安裝 [Composer](https://getcomposer.org/)
簡單介紹一下 composer 的用處，npm 是用來安裝 node.js 的套件管理工具，那麼 composer 就是用來安裝 php 套件的管理工具。
windows 這一步很簡單，按這裡就可以下載，設定那些一路按下去就好。
{% img /images/laravel-installation/1.png 800 200 laravel-installation %}
{% img /images/laravel-installation/2.png 800 200 laravel-installation %}

# 安裝 Ubuntu & Windows terminal
兩個都可以到 Microsoft store 下載。
{% img /images/laravel-installation/6.png 800 200 laravel-installation %}
{% img /images/laravel-installation/7.png 800 200 laravel-installation %}

# 安裝 WSL2
Laravel 環境有要求：
{% colorquote info %}
在新建 Laravel 應用前，請確保你的 Windows 電腦已經安裝了 Docker Desktop。
之後，請確保已經安裝並啟用了適用於 **Linux 的 Windows 子系統 2（WSL2）**。
WSL 允許你在 Windows 10 上執行 Linux 二進位制檔案。
關於如何安裝並啟用 WSL2，請參閱微軟 開發者環境檔案。
{% endcolorquote  %}
補充一下 WSL2 在幹嘛，看不懂就略過 XD：
{% blockquote %}
WSL 1 使用了一個轉譯層（translation layer）來轉換 Linux 與 Windows 底層的系統呼叫（system calls），而 WSL 2 已經不再需要這個轉譯層，因為它有了自己的 Linux 核心，而這個核心是執行於一個輕巧版本的 Hyper-V hypervisor 之上。
{% endblockquote %}
看一下安裝大師這個影片就懂了：
{% youtube wJUHe4iof7w %}
看完影片可以優先執行以下步驟來安裝 Windows WSL2，注意！要使用 windows powershell 並且以管理員身分來執行指令：
{% img /images/laravel-installation/5.png 800 200 laravel-installation %}

### 安裝 Linux 子系統 WSL2 步驟 1
``` bash
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
### 安裝 Linux 子系統 WSL2 步驟 2
``` bash
dism.exe /online /enable-feature /featurename:VirutalMachinePlatform /all /norestart
```
重啟電腦後下載影片提供的 Windows 補丁 WSL2 Linux kernel update package for x64 machines 才能執行新的 Linux 子系統 以及 Ubuntu 作業系統！下載之後安裝起來就可以囉！
{% img /images/laravel-installation/8.png 800 200 laravel-installation %}

### 查看系統版本
{% img /images/laravel-installation/12.png 800 200 laravel-installation %}

# 安裝 Docker
Docker 4.5.0 版本裝完之後會出現設定區塊不斷讀取轉圈圈的 Bug，這裡在 windows 是一個巨坑！ 

### 問題剖析
Docker 轉圈圈類似這樣，借別人的圖來演示一下：
{% img /images/laravel-installation/3.png 800 200 laravel-installation %}
沒有辦法將 Ubuntu 與 WSL2 的設定開起來，讓人從開始就決定放棄 XD
你一定在想為什麼還要用到 Ubuntu 與 WSL2？
上面有提到，Laravel 框架需要依賴 Linux 的 Windows 子系統 2（WSL2）才能執行，所以安裝 WSL2 在先而 Docker 應該在後，一開始就是順序錯誤踩了大坑跑不起來。
查看這一篇 [Docker does not work after installation #12545](https://github.com/docker/for-win/issues/12545) 發現我不孤單，這裡有提到可以先降版安裝 4.4.2 的 docker，裝好之後再 update 就可以解決轉圈圈的問題了！
{% blockquote %}
After I downloaded 4.4.2 it worked on the first try (well second try but basically all I did was restart it. There was some different error the first time and i clicked continue or ignore or try again or something. Anyways, it just then started. The WSL2 integration option box was still grayed out but it was selected so I didnt need to change it anyways. Then I did try to update to 4.4.4 but it actually installed 4.5.0. But it worked fine and my container was still there and working fine. So it seems that by installing 4.4.2 and then updating to 4.5.0 worked ok.
{% endblockquote %}

### 下載降板 Docker 4.4.2
[Docker](https://docs.docker.com/desktop/windows/release-notes/#docker-desktop-442) 官方釋出的版本號裡面下載 4.4.2。
{% img /images/laravel-installation/4.png 800 200 laravel-installation %}

### 更新 Docker 4.5.0
安裝完畢記得到 settings 更新到 Docker 4.5.0 版本。
{% img /images/laravel-installation/10.png 800 200 laravel-installation %}

### 修改設定
更改設定為 WSL2 並且**開啟 Ubuntu**
{% img /images/laravel-installation/11.png 800 200 laravel-installation %}
到這裡總算收拾好了一個 windows 才有的大坑阿~（累）

# 安裝 Laravel & sail
瞧瞧這份 [Laravel](https://laravel.com/docs/8.x/installation#getting-started-on-windows) 官方安裝文件，
我以為在 vscode 命令行輸入這一句就可以搭載 Laravel 了！真的是好傻好天真呀！全忘了剛才安裝的 Ubuntu XD
``` bash
curl -s https://laravel.build/example-app | bash
```

### 使用 Ubuntu 系統 sail up
在命令行輸入就會發現指令怪怪的，必須要去剛才安裝好的 Windows terminal 點選下拉符號的企鵝（剛剛下載的 Ubuntu作業系統）：
{% img /images/laravel-installation/9.png 800 200 laravel-installation %}
就可以在這裡使用剛才的命令行來安裝 Laravel & sail 
``` bash
./vendor/bin/sail up
```
到這裡終於可以啟航啦~（痛哭）
啟航沒多久你可以看到很多 Laravel 的專案被啟動，然後就報錯了～（崩潰）

### 問題剖析
錯誤訊息是這樣的：
{% colorquote danger %}
docker: Error response from daemon: Ports are not available: listen tcp 0.0.0.0:80: bind: An attempt was made to access a socket in a way forbidden by its access permissions.
{% endcolorquote %}
也就是我嘗試開啟的 Laravel Port 號被佔用了！指令查看一下 docker 容器列表：
後來刪除 80 port 也無法解決問題
``` bash
docker container ls
```
在電腦查找了一下，發現是 window 系統執行的某程式（編號4）佔用了
``` bash
netstat -ano | findstr 0.0:80
```
{% img /images/laravel-installation/13.png 800 200 laravel-installation %}
ctrl + shift + esc 啟動工作管理員 > 詳細資料 > 使用 PID 排序查找是甚麼系統佔用：
{% img /images/laravel-installation/14.png 800 200 laravel-installation %}

### 修改 Laravel 環境檔
不敢亂刪執行的系統背景程式，只好修改 Laravel 本身的 docker port 號，這裡要感謝課堂上的小夥伴們集思廣益查到方法了，參考 [Unable to set the APP_PORT on .env for Laravel Sail](https://stackoverflow.com/questions/67053449/unable-to-set-the-app-port-on-env-for-laravel-sail)
將專案的 .env 加上這一行，關閉在打開：
``` bash
APP_PORT=3000
```
{% img /images/laravel-installation/15.png 800 200 laravel-installation %}
``` bash
sail down
```
``` bash
./vendor/bin/sail up
```
就成功了！整個很想大哭一場 QQ
{% img /images/laravel-installation/16.png 800 200 laravel-installation %}

### 修改指令碼
最後修改一下啟動的指令更簡短些：
``` bash
alias sail='bash vendor/bin/sail'
```
就可以不必輸入那麼長的指令啟動 Laravel 啦！
``` bash
sail up
```
打完收工。

參考文章：
{% blockquote %}
https://forums.docker.com/t/docker-engine-wsl2-stopped-settings-page-doesnt-load/82355
https://github.com/docker/for-win/issues/12545
https://www.youtube.com/watch?v=wJUHe4iof7w
https://ek21.com/news/3/19760/
https://stackoverflow.com/questions/67053449/unable-to-set-the-app-port-on-env-for-laravel-sail
還要感謝課程上的死神老師與許多小夥伴們大力幫忙（跪）
{% endblockquote %}

____
2022/04/04 更

# Artisan cmd
Laravel 的 `artisan` 指令有些需要 **PHP 8 以上版本** 才能支援，需要更新環境的 PHP 版本到 8 以上，並且移除 PHP 7 避免系統指定到舊版本，這樣一來 windows 系統中的 windows terminal 與 ubuntu 企鵝都可以使用各種 artisan 指令，例如自動建立 Controller：

{% codeblock artisan cmd lang:JavaScript  %}
  php artisan make:controller Whatever Controller
{% endcodeblock %}

建立完的檔案會自動出現在 app/Http/Controllers 路徑中，結構如下：

{% codeblock WhateverController.php lang:JavaScript  %}
  <?php

    namespace App\Http\Controllers;

    use Illuminate\Foundation\Auth\Access\AuthorizesRequests;
    use Illuminate\Foundation\Bus\DispatchesJobs;
    use Illuminate\Foundation\Validation\ValidatesRequests;
    use Illuminate\Routing\Controller as WhateverController;

    class Controller extends WhateverController
    {
        use AuthorizesRequests, DispatchesJobs, ValidatesRequests;
    }

{% endcodeblock %}
