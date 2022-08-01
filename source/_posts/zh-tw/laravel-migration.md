---
title: Laravel 資料庫版控工具 Migration
date: 2022-04-24 19:18:30
tags:
- laravel
- migration
- sql
categories: laravel
---
{% img /images/laravel-migration/0.png 800 200 laravel-migration %}
Migration 位於 Laravel 專案包中的 database 資料夾，會使用建立日期來歸檔資料表的結構，為紀錄資料庫版本的版控，方便團隊共同開發資料表。

# 建立資料表
{% codeblock 建立 migration lang:JavaScript %}
  php artisan make:migration create_users_table
{% endcodeblock %}
建立成功會顯示：
{% img /images/laravel-migration/1.png 800 200 資料庫版控工具 Migration %}
重整之後會出現新的版控檔案，使用建立日期來命名：
{% img /images/laravel-migration/2.png 300 200 資料庫版控工具 Migration %}
檔案內容：
{% img /images/laravel-migration/3.png 800 200 輸入圖片資料夾名稱 %}

# 修改資料表

在 `\\wsl$` 路徑下執行 migrate 的坑需要以下步驟：
#### 補上 php 8.0 的 sql-extension
於 ubuntu 終端機輸入以下指令：
{% codeblock lang:JavaScript %}
輸入code
{% endcodeblock %}
#### 將註解 ;extension=pdo_mysql 打開