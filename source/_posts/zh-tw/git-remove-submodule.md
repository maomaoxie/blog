---
title: git 移除 submodule
date: 2022-08-07 16:07:24
tags:
- hexo
- git
- submodule
categories:
- git
---

{% img /images/git-remove-submodule/0.png 800 200 git-remove-submodule %}
# git submodule
在部屬 hexo 部落格到雲端 github 時，發現主題下的 minos git status 呈現 untracked 的狀態，搜尋後才知道該主題是 clone 遠端下來的，自動會變成一個子模塊（submodule），對於習慣專案開發的我來說在客製化後不能同步實在很不方便，還是喜歡一整包放在同一個 repository 方便管理。
{% img /images/git-remove-submodule/1.png 800 200 git-remove-submodule %}

以下記錄自己如何將修改後的 minos 資料夾：

### 移除 git 子模塊快取
以下的指令可以清除子模塊的 git 紀錄：
{% codeblock git cmd lang:git %}
  git rm --cached -f ./themes/minos
{% endcodeblock %}

### 重新加入索引
將剛才取消子模塊的 git 重新排隊至專案包中，我比較懶惰直接整包加：
{% codeblock git cmd lang:git %}
  git add .
{% endcodeblock %}

再檢視一遍 vs code 之後就會看到檔案被追蹤到了！