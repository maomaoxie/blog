---
title: RESTful API 方法介紹 - idempotent
date: 2022-08-09 15:14:24
tags:
categories:
---

RESTful API 呼叫的方法中，離不開四個主軸 CRUD，CREATE、READ/RETRIVE、UPDATE 還有 DELETE，而其中就有分是否為 Idempotent 的方法，甚麼意思呢？

{% blockquote %}
    A request method is considered "idempotent" if the intended effect on the server of multiple identical requests with that method is the same as the effect for a single such request. 
{% endblockquote %}

看為只覺得在母鯊大，沒關係我們繼續看下去：

{% blockquote %}
    Of the request methods defined by this specification, PUT, DELETE, and safe request methods are idempotent.
    來源：https://lance.coderbridge.io/2021/06/06/what-is-safe-method-and-indempotent-methods/
{% endblockquote %}

簡單來說簡單請求就是 Idempotent 的方法，不管你做一次跟一百次對於伺服器端資料改動的結果都一樣，是安全而沒有副作用的，可以安心服用；
相反的非 Idempotent 的方法則是需要謹慎考慮的、會改動伺服器資料庫的、有副作用的，也就是非簡單請求。

# idempotent methods
1. GET
2. HEAD, PUT, and DELETE

