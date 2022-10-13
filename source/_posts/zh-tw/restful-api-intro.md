---
title: restful-api-intro
date: 2022-09-28 10:42:52
tags:
categories:
---

RESTful API 的簡單介紹：

# 統一資源對外傳輸的接界窗口
可以透過資源分類命名，例如 https://www.example.com/books 就是關於書的相關 API 資料

# API 端口彼此獨立，不須互相依賴
資源按照端口分開接收，沒有次序或是參數依賴的問題

# 從 Client 端發送需求（request）
API 的資源傳送是分離式的，client 裝置發送 request 而 server 提供資源

# 具有 cache 機制
可以在 API 資源第一次返回時下載在客戶端裝置，存取在瀏覽器等暫時記憶位置，避免耗時重複讀取相同的資源，例如網站圖片或靜態資源

# 可架接中間層系統
在 API 流量評估的中間層設置負載平衡機制，適時的管控與擴充當下 API 使用情況以免伺服器負荷過大