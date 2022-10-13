---
title: Google Tag Manager 起手式
date: 2022-10-05 09:46:48
tags:
categories:
---

GTM（Google Tag Manager）是集合了 Google Analaytics、Google ADs、facebook pixel 等追蹤碼片段的一個總攬平台，
可以更方便的管理放置在網站上所有的追蹤行為，是這三大追蹤技術的綜觀介面，甚至可以使用這個 UI 平台放置更多的追蹤碼而不需要手動埋 code，
（第一步放置 GTM 追蹤碼還是需要的），是身為行銷、網頁工程師應該知道的追蹤技術。

# GTM 起手式
首先需要將**網站（website）當作一個容器**來啟動 GTM 帳號，並且將追蹤 code 放置在網站每一頁的 `<head>...</head>`以及 `<body>...</body>`，
完成 GTM 的安裝，並且使用 chrome 的外掛程式來確保安裝完成！
{% img /images/google-tag-manager-start-up/1.png 800 200 google-tag-manager-start-up %}

# 輔助工具
可以安裝 chrome 的 Google 行銷工具用的外掛（plugin），Tag Asistant Legacy（by google）在測試網站上點選錄製（Record）後，
過段時間偵測是否有安裝成功！
{% img /images/google-tag-manager-start-up/2.png 800 200 google-tag-manager-start-up %}
