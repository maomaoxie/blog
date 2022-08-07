---
title: 開始我的第一個 wordpress 網站
date: 2022-08-07 10:02:29
tags:
- wordpress
- dns
- cdn
categories:
- 
---

# 第一步 - 產生 ip 位址
首先要租賃一台伺服器提供 ip，可以考慮較優惠的主機廠商：遠振或 bluehost。
要注意的是，一個 domain 可以放置很多網站資料夾，並且設定sub-domian。

# 第二步 - DNS 註冊
ip 設定好之後就可以將網域指過來指定的 DNS 網域商：
中華電信提供的 .tw　較便宜，而 godaddy 則是 .com 較便宜。

# 第三步 - cdn 分散流量
常用的 cdn 廠商像是 cloudflare，可以註冊一個離主機近的（例如東京）cdn 位址，其他供應商例如 sugarhost 很便宜但連線較慢。

### CDN 原理
cdn 是把 DNS 轉向給 DNS 廠商（cloudflare），透過 DNS 指向將流量倒過去 cloudflare，在第一次 cache 過網站靜態資源後，第二次就可以直接提取cdn 伺服器上 cache　的資源以減少流量費用。

### 設定細節
##### 資源類型
通常都會設定 **A 類型**以設定網址（url）指向，例如 mailgun 的寄信功能。

##### 防堵資安問題
cloudflare 設定其中一樣可以開啟 ddos -> under Attack 來保護網站資源。
也可以手動清除特定檔案，例如圖片的 cache 來重新抓取檔案。

# 第四步 - 掛載 wordpress
wordpress 可以當作是一個 php 建構而成的後端框架應用，串接方式有兩種：
1. 直接使用 wordpress hook 來渲染資料（前後端合併）。
2. 撰寫自己的前端框架（例如 vue），撰寫自己的 API（前後端分離）。

### 建立步驟
到  [wordpress](https://tw.wordpress.org/download/) 官方網站下載檔案，上傳到自己的網域 server。
{% img /images/wordpress-get-started/14.png 800 200 wordpress-download %}
上傳後打開網站，會有一個 UI 介面可以直接設定 wordpress 與網站資料，config 需要設定資料庫帳號密碼：

要注意的是 wordpress 權限身分須從 root 改成 www-data，才能變成網站系統管理員使用 wordpress 後台。

行情 $ 25000 (使用佈景主題)
客製化算一頁
定時備份以免網站被駭客
租 GCP 靜態硬碟 
google 硬碟版本管理 資源回收桶保留30 天
flicker

設定 config 檔案 
比較好的後台c panel 要錢
dns 代管要 https 要使用 flexible SSL for cloudflare

一類有分類的文章 slug 需要代稱(英文的)
獨立頁面 客製化
siteliner 檢查有沒有壞掉的連結與重複的內容
duplicate page
報價高一點
附加的css
減少做重複的事情
在 wordpress 上做前後端分離
