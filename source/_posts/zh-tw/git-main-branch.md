---
title: Github 主分支更名 master 為 main
date: 2022-02-17 20:59:44
categories: git
tags:
- git
- github
- sourceTree
---

{% img /images/git-main-branch/1.png 800 200 git-main-branch %}
每每 github 搞了一些新招就得重新適應一次（菸

這兩天在做小專案 git init 之後準備推上去 github 時，執行以下的老樣子：
``` bash
git remote add origin https://github.com/mawchu/baby-bill.git
```
看到這裡不免疑惑了一下，不是印象中的 branch 名字呀：
``` bash
git branch -M main 
```
果然執行之後出現噴錯的內容：
{% colorquote danger %}
error: refname refs/heads/master not found
fatal: Branch rename failed
{% endcolorquote %}

這個 main 分支從哪裡冒出來的？暗暗覺得這個行為很雞肋= =？
{% colorquote info %}
為響應黑人平權運動，GitHub宣布從10月1日起改變新Git儲存庫的預設命名，以main來取代原本的master。
受到年中佛洛依德（George Floyd）遭警方執法過當死亡，引發的黑人平權抗議風潮影響，美國科技界也相繼思考去除慣用名稱中有種族歧視色彩的字眼，例如不要使用master/slave、blacklist/whitelist。GitHub執行長Nat Friedman也決定進行相關改變。

開發人員也可以不要變更，隨時到設定區，把個人、組織和公司的新儲存庫預設命名從main改成別的。
GitHub呼籲用戶可以先保持不動，到了年底會再釋出新工具以協助現有branch改成新的預設名稱。
在支持黑人平權風潮下，包括Google、微軟、IBM、Red Hat、甲骨文下的MySQL及Linux社群都相繼變更了軟體流程的命名。本月微軟也提案修改Chromium中black/whitelist為較中性的blocklist/allowlist。
https://www.ithome.com.tw/news/140094
{% endcolorquote %}
查看了一下網路說明，原來是美國的政治與歷史因素，生活跟科技真是息息相關呢，自己的見識尚淺默默反省了一下XD
科普了一會怎麼切換現今的分支變成 main：
``` bash
git add .
```
``` bash
git commit -m "要推的備註內容"
```
都 OK 就可以推上去囉！
``` bash
git push -u origin main
```
推上去發現自己忘了切換帳號又噴錯啦！
{% colorquote danger %}
remote: Permission to mawchu/baby-bill.git denied to maomaoxie.
fatal: unable to access 'https://github.com/mawchu/baby-bill.git/': The requested URL returned error: 403
{% endcolorquote %}
由於 git bash 切換帳戶不是那麼方便，就偷吃步一下重複 [Github 登入新制 PAT X Sourcetree](http://maomaoxie.github.io/2022/02/10/zh-tw/git-pat/) 這篇的步驟就可以囉！

參考文章：
{% blockquote %}
https://blog.csdn.net/taoshihan/article/details/116824815?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_default&utm_relevant_index=1
{% endblockquote %}