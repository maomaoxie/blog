---
title: HTTP 超文本傳輸協定
date: 2022-02-28 10:07:23
tags:
- http
- https
- request
- response
categories: http
---

{% img /images/http-and-https/0.png 800 200 [http-and-https [http-and-https]] %}
圖片來源：https://www.itpro.com/web-browser/33349/slew-of-vulnerabilities-detected-in-https

# HTTP 的誕生
網路應用程式早期在互相傳遞資料時沒有固定的格式，在資料解析上就會很麻煩混亂，就像一個國家一旦陷入各自為政的困擾就需要政治來規範管理，網路世界也是一樣道理的。超文本傳輸協定(Hyper Text Transfer Protocol)就是網路資料傳遞的統整者，是 client 端向 server 端要求資料的關卡，必須要按照正確的格式才能取得資料。

# Origin Server 是甚麼
### server 負責 response
先介紹 Origin 這個單辭為什麼會被拿來當作 server 端，它的同義詞有 root、source 與 beginning，有起源、來源與原產地的意思，相當於網路世界中網站資料的起源與來源，github 在推 commits 上去遠端也可以看到指令：
{% codeblock lang:JavaScript %}
  git push -u origin master
{% endcodeblock %}

Origin 代表的就是擁有資料的角色，通常是 server端：
{% blockquote %}
  An origin server is a computer that runs programs designed to listen to and respond to incoming requests or traffic. It contains the original version of the web page and is responsible for delivering the content to end users when requested.
  Origin servers take in requests and serve up content for a website or web page.
{% endblockquote %}
- Origin server 是負責監聽以及回應（response）來自四面八方的要求（requests）與通訊的電腦計算機程式，包含原版的網頁資料並且傳遞該內容給發出請求的使用者端口。
- Origin server 會接收需求並且提供網站的資料內容。

### client 負責 requested
Client 代表的是上網並請求網站資料的使用者，也就是 user 端：
{% blockquote %}
  When a user opens a web page on a website, a request is sent to the origin server to retrieve the content.
{% endblockquote %}
- 當使用者試圖用瀏覽器（browser）打開一個網頁或網站，就已經發送了該網站資料的要求給 origin server。

# Port 是甚麼
一個網站就如同銀行，而服務人員的櫃檯提供多元的銀行業務來消化前去等候的民眾，這個案例中：
銀行機構就是 Origin server，服務人員就是 port，而民眾就是 client（user agent）；而一個網站 Origin 可以包山包海擁有各種資料，並開啟多個窗口的埠號（port）來服務傳送過來的需求（requests），讓每個埠號更專注在一個服務的品質與細節上。
早期沒有瀏覽器的時代，使用的是終端機(terminal)來發送請求而非瀏覽器，而今 server 端口會檢查要求是否來自瀏覽器而非機器人（爬蟲），來過濾一些資安問題的黑手。

# HTTPs 的誕生
隨著網路世界日興發達，不安好心的資訊竊取駭客就會出現，它可以偽裝成使用者也可以偽裝成服務器，為了避免這種資料截取的資安問題，資訊傳遞的加密就很重要！HTTPS 如同 HTTP 但是多了一把開啟資訊傳遞的鑰匙，將來往資料都加密過防止有心人士解讀：
{% img /images/http-and-https/1.png 600 200 [http-and-https [http-and-https]] %}


參考資料：
{% blockquote %}
  https://www.cdnetworks.com/knowledge-center/what-is-origin-server/
  hiskio 課程 API 整合實戰｜RESTful 第三方串接應用
{% endblockquote %}