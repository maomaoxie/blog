---
title: RESTful API 方法觀念介紹 - idempotent
date: 2022-08-09 15:14:24
tags:
categories:
---

RESTful API 呼叫的方法中，離不開四個主軸 CRUD，CREATE、READ/RETREIVE、UPDATE 還有 DELETE。
而其中就有分是否為 **Idempotent 的方法**，甚麼意思呢？

# 甚麼是Idempotent（冪等）
{% blockquote %}
    A request method is considered "idempotent" if the intended effect on the server of multiple identical requests with that method is the same as the effect for a single such request. 
{% endblockquote %}

看為只覺得在母鯊大，沒關係我們繼續看下去：

{% blockquote Lauviah0622 https://lance.coderbridge.io/2021/06/06/what-is-safe-method-and-indempotent-methods/ 
[極短篇] HTTP 的 Safe method 還有 Idempotent method%}
    Of the request methods defined by this specification, PUT, DELETE, and safe request methods are idempotent.
{% endblockquote %}

dempotent（冪等）的方法不管你做一次、兩次乃至一百次，對於伺服器端資料的結果都一樣，是安全而沒有副作用的，可以安心服用；
相反的非 Idempotent 的方法則是需要謹慎考慮的、會改動伺服器資料庫的、有副作用的，也就是非安全請求。

我們來看看更精簡好懂的解釋：

{% blockquote James E. https://blog.dreamfactory.com/what-is-idempotency/ what-is-idempotency %}
    Idempotent operations produce the same result even when the operation is repeated many times. The result of the 2nd, 3rd, and 1,000th repeat of the operation will return exactly the same result as the 1st time.
    冪等運算是指無論操作多少次結果都會與第一次相同。

    For example, simple mathematical examples of idempotency include:

    x + 0;
    x = 5;

    In the first example, adding zero will never change the result, regardless of how many times you do it. In the second, x is always 5. Again, this is the case, regardless of how many times you perform the operation. Both of these examples describe an operation that is idempotent.

    以上兩個例子都說明了這兩個表達是無論執行幾次都會是相同結果，這就是 Idempotent（冪等）。
{% endblockquote %}

# 冪等不等於安全請求
{% blockquote James E. https://blog.dreamfactory.com/what-is-idempotency/ what-is-idempotency %}
    The concepts of ‘idempotent methods’ and ‘safe methods’ are often confused. A safe method does not change the value that is returned, it reads – but it never writes.
    Therefore, all safe methods are idempotent, but not all idempotent methods are safe.

    HTTP methods include:
    POST – Creates a new resource. POST is not idempotent and it is not safe.
    GET – Retrieves a resource. GET is idempotent and it is safe.
    HEAD – Retrieves a resource (without response body). HEAD is idempotent and it is safe
    PUT – Updates/replaces a resource. PUT is idempotent but it is not safe
    PATCH – Partially updates a resource. PATCH is not idempotent and it is not safe.
    DELETE – Deletes a resource. DELETE is idempotent but it is not safe.
    TRACE – Performs a loop-back test. TRACE is idempotent but it is not safe.
{% endblockquote %}

安全請求只會讀取，所以都是冪等的，但冪等方法不一定都是安全請求。

學術的部份我們就此打住，了解一下冪等之於 RESTful API 的意義。

# idempotent methods 冪等的方法
1. GET
2. HEAD（只讀取資料頭而忽略身體）
3. PUT
4. DELETE
5. OPTIONS

以上方式無論發幾次 request，結果都等同於一次。

{% blockquote Lauviah0622 https://lance.coderbridge.io/2021/06/06/what-is-safe-method-and-indempotent-methods/ 
[極短篇] HTTP 的 Safe method 還有 Idempotent method%}
    通常 DELETE 會帶上 id，所以刪除 1 次和刪除 100 次是一樣的，server 那邊找不到 id 操作就會被忽略。
    而 PUT 也一樣，PUT 代表替代的 http 操作，你發了 1 次 request 已經取代了內容後，那即使再發 100 次也只是替代一樣的內容。
{% endblockquote %}

# Not idempotent methods 非冪等的方法
1. POST
2. DISPATCH

以上方法每執行一次就會造成資料變動，但並非每種使用方式都是非冪等，端看 request 的目的是在「修改」還是「增加」，修改可能並不會更改內存量（memory），但是增加就不同了，它也是修改但是擴大了內存量（變多了）：
{% blockquote Lauviah0622 https://lance.coderbridge.io/2021/06/06/what-is-safe-method-and-indempotent-methods/ 
[極短篇] HTTP 的 Safe method 還有 Idempotent method%}
    PATCH 在語意上代表著修改資料，換句話說可能這樣：

    `PATCH http://blog.com/post?id=1
    body
    {
        title: 'new title'
    }
    `
    發了 100 次和 1 次標題都是同樣的 new title。

    `
    PATCH http:shop.com/item/add?id=1
    body
    {
        number: 10
    }
    `
    requst 代表的是增加 10 個 item 的數量。這種情況下也符合語意（修改資料），但就不符合 Idempotent 了，100 次會新增 1000 個。那 POST 就不用提，一次和 100 次肯定是不一樣的。
{% endblockquote %}

# 結語
讓我最意外的是 DELETE 居然是冪等方法，原因在於無論 request 幾次都只刪同一筆資料（認 id）這個觀念，與以前認為 DELETE 應該會每次刪除不同資料的想法大相逕庭。