---
title: Web Socket v.s Web Hook
date: 2022-02-25 11:29:08
tags:
- WebSocket
- WebHook
- api
categories: webSocket
---
{% img /images/web-socket-and-web-hook/0.png 800 200 [web-socket-and-web-hook [web-socket-and-web-hook]] %}
API 的形式有兩大宗：
Web Hook 與 Web Socket，而他們有甚麼差異呢？

# 被動式 Web Hook
例如 Line Bot，server 會先預測使用者的動作與需求（events），設計出相對的程式並維護，等待使用者發出要求才動作。

# 主動式 Web Socket
兩方的資料交互，例如 client 與 server、server 與 server。
應用場景如聊天功能：facebook messenger 或 Line 等，Web Socket 的技術可以「即時的」(real time)做出資料傳遞。
當你打開 facebook messenger 的視窗，Web Socket server 就會與 client 瀏覽器端 handshake 保持通訊暢通，持續監聽對話要求並回應(opened and persistent connection)

參考資料：
{% blockquote %}
  hiskio 課程 API 整合實戰｜RESTful 第三方串接應用
{% endblockquote %}