---
title: HTTP 的訊息頭 header
date: 2022-02-28 20:28:24
tags:
- http
- httpHeader
- request
- response
categories: http
---
{% img /images/http-header/0.png 800 200 [http-header [http-header]] %}

個人對這一塊知識又愛又恨，每每串接 api 打開 Network 都會看到卻似懂非懂的感覺真討厭！來認真釐清一下吧！

# 甚麼是 Header 與 Body
訊息頭`header`在 request 與 response 裡佔有很大一席之地，它的作用相當於寄件時包裹上的訊息，記錄著包裹的相關內容，譬如該包裹要通過海關時必須申報內容物是甚麼類別，是否是急件還是一般，要寄往哪個國家、寄給誰等等的屬性。
訊息本體 `body` 就是你包裹裡面的東西，或者信件裡的文字訊息。
之於網頁的資料傳遞則是記錄該次要求 request 與回應 response 的通訊內容．是一個 http 行為的概覽與說明。
___

# Header 存在於那裡
request 與 response 都有 `header`，通常是 client 端發出的 request 會寫得比較詳盡，譬如使用者在搜尋引擎上點選一個網站就完成數個 request（一個網站會有很多資源的要求．例如圖片、網頁或 Api 資料）或者使用者在電腦填寫表單之後送出，而 server 端接收之後回應也會有訊息頭。

___
# HTTP 通訊的組成
{% img /images/http-header/1.png 500 200 http-header %}
一個 request 跟 response 由 `header` 訊息頭跟 `body` 訊息本體所組成：

### Request Header 請求頭
| 屬性               | 說明                           | 範例                             |
| :----------------- | -----------------------------: | :-----------------------------: |
| **Accept**             | request 要求 response 的類型    | text/html, image/*              |
| **Accept-charset**     | request 的編碼                 | utf-8, iso-8859-1;q=0.5         |
| **Authorization**      | request 要通過的驗證授權檢查    | Basic YWxhZGRpbjpvcGVuc2VzY...   |
| **Cookie**             | client 的暫存資料              | PHPSESSID=124141;csrftoken=45746454   |
| **Content-type**       | request 要求所發送的資料型態    | multipart/form-data, text/plain, application/x-www-form-urlencoded, text/html, application/json   |
| **Origin**             | request 要求的資料機器來源      | https://developer.mozilla.org   |
| **User-Agent**      | request 端的 client 資訊    | Mozilla/5.0(Macintosh;Intel Mac OS X_10_14_3) AppleWebKit/537.36 |
| **X-Csrf-Token**      | 防止 csrf(跨站請求偽造) 攻擊    | eyjpdil6lmc4bjhQMmFrays... |

### Authorization
其中的 `Authorization` 在 API 的**安全驗證**非常重要，當 API server 接收到來自遠方的 request 時，可以透過該鑰匙密碼確認來源的 request 是合法、通過認證的，如此才能通過關卡進行資料交換。

### Cookie
瀏覽器的**暫存資料**，是 Web API 的一種，Cookie 提供前端或後端一個存取少量資料在瀏覽器的空間，資料格式是一個鍵值對(key-value)，常用來做廣告追蹤紀錄客戶的瀏覽喜好，進而針對客群行銷與增加廣告投放的精準度，例如 Google Analytics；亦或是用來保持使用者的登入狀態。
{% blockquote %}
身為 Web 開發人員一定要瞭解 **HTTP 本身無狀態 (Stateless) 的特性**，要在網路上識別瀏覽者的身份，必須透過一些機制來保存狀態，而 Cookie 就是其中一種保存狀態的機制。
https://blog.miniasp.com/post/2008/02/22/Explain-HTTP-Cookie-in-Detail
{% endblockquote %}
{% blockquote %}
Cookies 會被每一個請求發送出去，所以可能會影響效能（尤其是行動裝置的資料連線）。現代客戶端的 storage APIs 為 Web storage API (en-US) （localStorage 和 sessionStorage）以及 IndexedDB。
https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Cookies
{% endblockquote %}

### Content-type
紀錄 client 端發出的 request 要求發送出去的**資料格式與型態**，例如表單或影音多元媒體就是 `multipart/form-data`，網頁就是 `text/html`，而 API axios 等技術傳遞的資料格式常為 `application/json`等。
{% blockquote %}
一般的 Content-Type 往往只能傳送一種形式的資料，但在網頁的應用當中我們還可能想要上傳檔案、圖片、影片在表單裡頭，這樣的需求促成了 multipart/form-data 規範的出現。
https://blog.kalan.dev/2021-03-13-html-form-data/。
{% endblockquote %}

### Origin
該 request 要求的來源，比如瀏覽的網站是 https://google.com 等，延伸的議題就是跨網站要求(CORS)，經常會默認要同一個**網站來源(domain)**伺服器端才可以接受該要求。

### User-Agent
發送 Request 的對象，通常是**瀏覽器本身的相關資訊**，例如 chrome 瀏覽器就會顯示 Mozilla/5.0 (Windows NT 10.0; Win64; x64)；也可能不是瀏覽器，例如 postman 就是 postman、終端機就是空白，`User-Agent` 主要是拿來防止爬蟲軟體等非瀏覽器或機器人來獲取網站資料。

### X-Csrf-Token
該 token 用以防止 csrf 的跨站偽造攻擊，使用者送出表單時加上一傳鑰匙密碼，避免有心人士撰寫一隻程式發送表單登入或註冊網站會員竊取資料。
___

# Response Header 回應頭
回應的內容也會記錄伺服器端的資料，以及接收的 request 資訊。
| 屬性                                   | 說明                                | 範例                                 |
| :------------------------------------ | ----------------------------------: | :----------------------------------: |
| **Access-Control-Allow-Origin**        | 限定此回應的資源是否要限制某個 Origin  | *, https://developer.mozilla.org    |
| **Server**                             | 後端 Web server 伺服器類型            | Apache/2.4.1(Unix), Nginx    |
| **Status**                            | 回報這次 request 是否成功的代碼     　　| sessionid=38afes7a8, id=a3fWa, Max-Age=2592000   |

### Access-Control-Allow-Origin
該技術常用來判定前後端分離的網站，後端 server 限定只能接受自己**官方網站(domain)**前端打出的 API Request，才會做出回應；該技術與 Request Header 提到的 `Origin` 前後呼應，由後端 server 判定前端 Request Header 裡面備註的 Origin 來源是否合法，延伸的議題一樣就是跨網站要求(CORS)。

### Set-cookie
伺服器端在瀏覽器設定 cookie 資料以記錄客戶端的瀏覽行為，以利行銷追蹤，或是做後續 request 發送給後端要包含的 cookie 內容。
而後端發送 Set-Cookie 是主動的，而前端接收 Cookie 是被動的：
{% blockquote %}
The Set-Cookie HTTP response header is used to send a cookie from the server to the user agent, so that the user agent can send it back to the server later.
{% endblockquote %}
{% blockquote %}
Cookie 的運作是這樣的：
1. **Server 端**回應給 Browser 一個或多個 "Set-Cookie" HTTP Header
2. Client 端 ( Browser ) 接收到 Set-Cookie 指令時，會將 Cookie 的名稱與值儲存在 Browser 的 Cookie 存放區，並記錄該 Cookie 隸屬的網域、網址路徑、過期時間、是否為安全連線
3. 當 Browser 再次發出 HTTP Request 指令到 Server 時，就會比對目前在 Browser 內的 Cookie 存放區有沒有「該網域」、「該目錄」、「過期時間尚未過期」且「是否為安全連線」的 Cookie，如果有的話就會包含在 HTTP Request 指令的 "Cookie" Header 中。
{% endblockquote %}
{% colorquote danger %}
如果Request 要傳送 Cookie 到後端
除了在 前端 Request 要設定 **credentials**
後端 Express 的 **Access-Control-Allow-Origin 不可以為'*'**

要設定成一個指定域名(前端的ip或域名)，例如：
Access-Control-Allow-Origin："http://localhost:3000"
{% endcolorquote %}

___
{% blockquote %}
圖片來源：hiskio 課程 API 整合實戰｜RESTful 第三方串接應用
{% endblockquote %}