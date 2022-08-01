---
title: Github 登入新制 PAT X Sourcetree
date: 2022-02-10 12:31:14
categories: git
tags:
- git
- github
- sourceTree
---
		
在2022年某天，嘗試使用 sourcetree 推上去新 code 時，在 Github 時出現了以下錯誤訊息：
{% colorquote danger %}
remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.
fatal: unable to access "..." : The requested URL returned error: 403
{% endcolorquote %}

以上說明大概就是 Github 已經不支援密碼登入的方式，從 2021/08/13 開始必須替換為 PAT 的 token 登入方式進行認證。

這個過程採了一些小坑，為了避免自己忘記趕緊的筆記一下：

# Github 創建 token
  {% blockquote %}
  From your GitHub account, go to Settings => Developer Settings => Personal Access Token => Generate New Token (Give your password) => Fillup the form => click Generate token => Copy the generated Token, it will be something like ghp_sFhFsSHhTzMDreGRLjmks4Tzuzgthdvfsrta
  {% endblockquote %}
  1. 按下右上角頭像，選取 settings
  {% img /images/github-pat/github-pat1.png 800 100 [title text [alt text]] %}
  2. 按下 developer settings
  {% img /images/github-pat/github-pat2.png 800 200 [title text [alt text]] %}
  3. 點選 personal accesss token
  {% img /images/github-pat/github-pat2-1.png 800 200 [title text [alt text]] %}
  4. 填寫 token 用途與勾選 token 權限之後，申請一個 token 並且妥善保存
  {% img /images/github-pat/github-pat2-2.png 800 200 [title text [alt text]] %}

# sourceTree 修改遠端 URL
  專案的 repo 點選 settings 修改 remote URL 的格式如下，
  ``` bash
  https://<USERNAME>:<TOKEN>@<GIT_URL>.git
  ```

  查詢 GIT_URL 可以透過這個指令：
  ``` bash
  $ git config --get remote.origin.url
  ```

  並且記得 GIT_URL 要刪除 https:// 的部分才不會出錯，類似這樣 
  ``` bash
  https://mawchu:<你的Github token>@github.com/mawchu/mawchu.github.io.git
  ```
# 驗證 Sourcetree 的身分
  {% img /images/github-pat/github-pat3.png 800 200 [title text [alt text]] %}
  {% img /images/github-pat/github-pat3-1.png 800 200 [title text [alt text]] %}

  切換 OAuth 為 Basic 驗證方式，輸入方才申請好的 token 密碼
  {% img /images/github-pat/github-pat4.png 800 200 [title text [alt text]] %}

  驗證 OK 就可以推上去囉！

參考文章：
{% blockquote %}
https://stackoverflow.com/questions/68775869/support-for-password-authentication-was-removed-please-use-a-personal-access-to
https://stackoverflow.com/questions/68191968/source-tree-fix-for-git-password-authentication-is-temporarily-disabled-as-part
https://stackoverflow.com/questions/4089430/how-can-i-determine-the-url-that-a-local-git-repository-was-originally-cloned-fr
{% endblockquote %}
