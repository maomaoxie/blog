---
title: 在 Vue2 + Typescript 專案中引入 WOW.js
date: 2022-08-25 11:00:37
tags:
categories:
---

# 安裝模組
{% codeblock lang:JavaScript %}
npm install wowjs
{% endcodeblock %}

# 引用 css
這一步要注意的地方是 animate.css 的使用方式，使用 npm install 安裝的 animate.css 會在 wowjs 套件包下的 `css/lib/animate.css`，而不是透過 npm 安裝的 animate.css：

### wowjs 套件包下的 animate.css
{% codeblock lang:JavaScript %}
import 'css/lib/animate.css';
{% endcodeblock %}

### npm 安裝的 animate.css
{% codeblock lang:JavaScript %}
import 'animate.css';
{% endcodeblock %}

{% colorquote danger %}
值得注意的點就是在於 css 名稱套用上的不同，wowjs 使用上不需要 animate 前綴，而 npm 安裝的需要，如果得搭配滾動視窗位置才套用動畫，需要注意 wowjs 套件包下的 `css/lib/animate.css` 引用模組方式。
{% endcolorquote %}

{% blockquote %}
[关于wow.js在vue项目中的使用及遇到的坑（css3效果）](https://blog.csdn.net/qq_32963841/article/details/115690823)
{% endblockquote %}