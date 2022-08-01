---
title: HTTP Status code 狀態碼解析
date: 2022-03-01 22:27:36
tags:
- http
- https
- status
categories: http
---
{% img /images/http-status-code/0.png 800 200 [http-status-code [http-status-code]] %}
圖片來源：https://alicialin2020.medium.com/status-code-%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98-4%E5%AD%97%E9%A0%AD%E7%B3%BB%E5%88%97-4826b7e07ae5

常常看到網站出現 404 找不到，或者是後端噴錯顯示了 Bad Gateway 卻不曉得箇中原因，而前後端打 API 也必須要讀懂狀態碼（HTTP Status code）代表的意涵，才能更有效率的除錯找出原因，以下是常見的狀態碼：


# Level 200 發送成功
| 狀態碼     | 意涵       | 情境                                   |
| :-------- | ---------: | :--------------:  |
| 200       | OK          | API 要求與回應都OK    |
| 201       | Created      | 成功建立了資料，例如 facebook 建立貼文    |
| 203       | Http proxy    | 該伺服器是代理的伺服器，非原始 Origin    |
| 204       | No Content    | 伺服器成功處理了請求，沒有返回任何內容   |

# Level 400 要求內容有誤
| 狀態碼     | 意涵             | 情境                                             |
| :-------- | ---------------: | :---------------------------------------------:  |
| 400       | Bad Request      | 發送要求但是需要的參數沒填、填寫的參數不符合規則，這個錯誤是客戶端造成的      |
| 401       | Unauthorized     | 身分未明或是瀏覽的權限不夠、沒有登入會員或付費        |
| 403       | Forbidden        | 黑名單而被禁止該項服務                              |
| 404       | Not Found        | 找不到該網站的某頁面                                |
| 409       | Conflict         | 服務流量過大導致衝突，或帳號/Email已被使用                               |

# Level 500 伺服器有誤
| 狀態碼     | 意涵                           | 情境                                   |
| :-------- | -----------------------------: | :--------------------------------------:  |
| 500       | Internal Server Error          | 該服務伺服器出現不明問題，需要聯絡開發人員    |
| 503       | Service Unavailable            | 該服務因為流量爆炸等原因無法使用了           |
| 501       | Not Implemented                | 該服務有接收到要求，但還未實作未完成         |
| 502       | Bad Gateway                    | 連線伺服器成功，但中間軟體出錯   　　　　　　|
| 504       | Gateway Timeout                | 連線伺服器成功，程式也沒問題，但經過中間軟體時出錯（過期）   　　　　　　|
| 599       | Network Timeout                | 網路連線逾時，連不上　　　　　   　　　　　　|

參考資料：
{% blockquote %}
  hiskio 課程 API 整合實戰｜RESTful 第三方串接應用
{% endblockquote %}

延伸閱讀：
{% blockquote %}
https://alicialin2020.medium.com/status-code-%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98-4%E5%AD%97%E9%A0%AD%E7%B3%BB%E5%88%97-4826b7e07ae5
{% endblockquote %}