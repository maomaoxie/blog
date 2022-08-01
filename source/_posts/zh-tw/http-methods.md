---
title: HTTP Methods 介紹
date: 2022-02-28 11:03:47
tags:
- http
- httpMethods
- api
- request
- response
categories: http
---

{% img /images/http-methods/0.png 800 200 [http-methods [http-methods]] %}
圖片來源：https://www.jianshu.com/p/ac29da71601d

身為技術人不可不知的技術事：其實你每天都在使用 HTTP 的 Methods，但你不知道而已。
當你在瀏覽器輸入一串網址並且按下 enter 或者是按下一個 google 搜尋的超連結，就已經完成一個 HTTP `GET` Methods 的流程了！
Client 端網路資料的傳輸，按照功能屬性分為幾個類別：

| Methods  | 目的         | 說明                                　　　　　　　　　　　　　　　   　 |
| :------- | :----------  | :----------------------------------------------------------------    |
| GET      | 取得資料      | 經常會搭配 query string 參數使用，使用者可以在 URL 看到資料             |
| POST     | 傳送資料      | 可以將要求內容隱藏在 body 中，使用者看不見                             |
| PUT      | 更改整筆資料  | 通常是一個註冊表單提供第一次來訪的使用者填寫，server 端會整筆更新         |
| PATCH    | 更改部分資料  | 可能是會員中心的部分資料修改，例如地址或手機，server 端會只更新修改的部分  |
| DELETE   | 刪除資料      | 比較單純的功能，就是刪除資料                                           |

___
# GET 方法（READ）
{% img /images/http-methods/1.png 500 200 [http-methods [http-methods]] %}
圖片來源：hiskio 課程 API 整合實戰｜RESTful 第三方串接應用

GET 取得資料（read）的條件是寫在 query string 的`?`後方，key-value 的鍵值對來代表欄位跟內容。
例如 `?name=jobe&email=jobe`，`name` 對應`form` 表單裡面的 input `name` ，為欄位名稱，等號後方的值就是使用者自己填寫的 value 內容：
{% codeblock lang:JavaScript %}
  <form action="http://localhost:4000">
    <div>
      <label for="name">Name</label>
      <input type="text" name="name">
    </div>
    <div>
      <label for="email">Email</label>
      <input type="text" name="email">
    </div>
    <button type="submit">送出</button>
  </form>
{% endcodeblock %}
{% img /images/http-methods/2.png 500 200 [http-methods [http-methods]] %}
{% img /images/http-methods/3.png 500 200 [http-methods [http-methods]] %}
{% colorquote info %}
  `GET` 網址最多只能兩千多個字，包含參數等。
{% endcolorquote %}
{% colorquote info %}
  URL 的萬國編碼機制需要經過 `encodeURI()`、`encodeURIcomponent()` 等方法解密過才可閱讀。
{% endcolorquote %}

___
# POST 方法（CREATE）
{% img /images/http-methods/4.png 500 200 [http-methods [http-methods]] %}
圖片來源：hiskio 課程 API 整合實戰｜RESTful 第三方串接應用

`POST` 方法旨在創造一筆新的資料，發送到 Origin server 接收，可以將資料隱藏在 `body` 中避免被使用窺視，使用者在網頁 URL 看起來也乾淨簡潔。
{% colorquote info %}
`POST` 方法可傳送的資料量基本沒有限制，端看 server 這邊的程式是否有特別限制多少 mb。
{% endcolorquote %}

___
# PUT & PATCH（UPDATE）
這兩個方法旨在更新現有的資料內容：
`PUT`會將整個表單內容根據填寫的資料全部更新，有空白沒填的地方照樣更新成空白的狀態，適合用在第一次填寫資料的訪客，例如註冊會員、初診的病患要掛號。
`PATCH`則是只更新部分內容而已，沒有變動的地方維持原樣，適合用在填寫過完整資料而只想部分更改的情境，例如修改會員寄送地址、復診的病患要掛號等。

這兩個方法並非 HTTP 創造的，而是方便網站開發人員依據自己的資料取向需求來設計表單欄位給訪客填寫，再選擇適合的 HTTP Methods。

___
# DELETE（DELETE）
由於刪除是一個不能回頭的行為，雖然有的網站在後端機制不會真的讓使用者刪除，只是讓你看不到而已，不過刪除的要求還是被特別獨立出來，警示開發人員慎用這個方法。

___
{% blockquote %}
圖片來源：hiskio 課程 API 整合實戰｜RESTful 第三方串接應用
{% endblockquote %}