---
title: Google Apps Script 輕鬆寫入表單
date: 2022-09-28 14:54:00
tags:
categories:
---

先前自己的一個小專案使用了 Google 官方的 Sheet Api v4 來寫入花費明細，除了要自幹一套 Node.js 搞定 Sheet API 的 OAuth 之外，還要忍受每過七天 tkn 就會失效的痛苦，終於在茫茫文件海中發現到 App 小天堂 Google Apps Script 這套輕量型的 Paas 平台，讓你撰寫一段小小的 javascript code 就能一秒產生 API endpoint！

# 啟用 GCP Google Apps Script
首先要準備好的就是你 GCP 上的 API 要記得啟用，打開 Google Sheets API 以及 APPs Script API：
{% img /images/google-apps-script-try/0.png 600 200 google-apps-script-try %}

打開之後去建立一個空白表單來產生需要的 Sheet ID，會在表單連結裡 `d/` 後方的一串亂碼．
{% img /images/google-apps-script-try/1.png 800 200 google-apps-script-try %}

接著把表單存取權限開到最大，讓任何知道連結的人都可以編輯表單：
{% img /images/google-apps-script-try/2.png 800 200 google-apps-script-try %}

# 透過 Google Apps Script 建立應用程式
## 撰寫程式
接下來是重頭戲，建立寫入 Google 表單的 app，點案這裡進入 Google Apps Script 介面，撰寫以下的程式碼：
來源之後補上：
{% codeblock lang:JavaScript %}
function doPost(e) {
  // 取得輸入參數
  let params = e.parameter; 
  let name = params.name;
  let mail = params.mail;
 
  // 初始化試算表
  let SpreadSheet = SpreadsheetApp.openById("輸入你斜線 d 後面的表單 id");
  let Sheet = SpreadSheet.getSheets()[0]; // 指定第一張試算表
  let LastRow = Sheet.getLastRow(); // 取得最後一列有值的索引值

  // 寫入試算表
  Sheet.getRange(LastRow+1, 1).setValue(name);
  Sheet.getRange(LastRow+1, 2).setValue(mail);
  
  // 回傳結果
  return ContentService
  .createTextOutput(JSON.stringify({ result: '成功', version: '1.0' }))
      .setMimeType(ContentService.MimeType.JSON); 
}
{% endcodeblock %}


## 部屬程式
點選右上方的部屬，選擇類型為「網頁應用程式」．以及呼叫 API 的權限設定要記得選擇，所有人都可以呼叫這個程式並且寫入：
{% img /images/google-apps-script-try/4.png 800 200 google-apps-script-try %}
{% img /images/google-apps-script-try/5.png 800 200 google-apps-script-try %}

## 獲得端點 API
部屬完成會有 API 網址產生，這時候就可以複製起來然後使用 postman 操作看看是否成功囉!
{% img /images/google-apps-script-try/6.png 800 200 google-apps-script-try %}
{% img /images/google-apps-script-try/7.png 800 200 google-apps-script-try %}

來查看一下 google sheet 這邊是否成功寫入：
{% img /images/google-apps-script-try/8.png 800 200 google-apps-script-try %}

大功告成囉!