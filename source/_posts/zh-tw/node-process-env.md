---
title: Node如何設置.env環境檔
date: 2022-02-21 14:48:33
tags:
- node
- env
- process
- dotenv
- git
categories: node.js
---
{% img /images/node-process-env/1.png 800 200 node-process-env %}
圖片來源:https://dev.to/aadilraza339/what-is-env-file-in-node-js-3h6c

開發專案的時候機密文件如何保存很重要，可以保護不被有心人士竊取，像是 API 金鑰、client-ID 與 client-secret 等可以呼叫遠端資料的通關密碼都需要妥善保存不暴露到網站上。
這邊來說明一下怎麼設置專案裡的環境檔案，並且讀取機密資料：

# 創建 .env 檔案
首先在跟目錄新增一個 .env 檔案，甚至可以設置以下幾種檔案來撰寫想要的環境模式腳本：

### 根據環境模式建立
- 全局環境檔：.env
- 開發環境檔：.env.dev
- 測試環境檔：.env.sit
- 生產環境檔：.env.prod

### 環境檔變數
內容很簡單，都是鍵值對（key-value）的方式撰寫變數：
{% codeblock .env %}
YOUR_VARAIBLE_NAME=VALUE
{% endcodeblock %}

# Node.js process
Node 提供了一個全局可使用的模組 process 記錄了所有關於目前 Node.js 程式的資訊，不需要 require()　就可以使用，棒棒的。
> The process object is a global that provides information about, and control over, the current Node.js process. As a global, it is always available to Node.js applications without using require().

### process.env
process.env 則提供了關於使用者的環境物件，而腳本檔案需要的資訊都可以放在這個物件中。
該物件中的資料會自動轉換成字串（string）。
在 windows 作業系統中環境變數的大小寫是不敏感的。
> The process.env property returns an object containing the user environment. 
> See environ(7). Assigning a property on process.env will implicitly convert the value to a string.
> On Windows operating systems, environment variables are case-insensitive.

# npm dotenv
光有全局的 process.env 是不夠的，還需要使用一個神人創造的 **npm 套件 [dotenv](https://www.npmjs.com/package/dotenv)** 才能無痛讀取自己創建的 .env 資料唷！
```
npm i dotenv --save
```
在專案引用該套件：
{% codeblock lang:JavaScript %}
  require('dotenv').config()
  const db = require('db')
  db.connect({
    host: process.env.DB_HOST,
    username: process.env.DB_USER,
    password: process.env.DB_PASS
  })
  https://dev.to/aadilraza339/what-is-env-file-in-node-js-3h6c
{% endcodeblock %}

# 善用 .gitignore
通常搭配 git 使用時會放置在 env 檔中並且 .gitignore 避免 commit 到公開環境中。
使用方式很簡單，在跟目錄創建一個 .gitignore 檔案之後，撰寫需要被 git 略過不被 commit 以及 push 的檔案類型：
{% codeblock .gitignore lang:JavaScript %}
  .env
  .env.dev
  .env.sit
  .env.prod
{% endcodeblock %}

參考資料：
> https://dev.to/aadilraza339/what-is-env-file-in-node-js-3h6c
> https://dwatow.github.io/2019/01-26-node-with-env-first/
> https://nodejs.org/docs/latest-v8.x/api/process.html#process_process_env