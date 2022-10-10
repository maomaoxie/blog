---
title: typescript-why-use-it
date: 2022-08-20 14:42:17
tags:
categories:
---

# Typescript is a superset of Javascript
Typescript 提升的重點就是：優化了你的原生 Javascript 型別提示。
Typescript 是根基在 Javascript 語言之上的一個超集，他提供更強大的特點並且優化了 Javascript 的缺陷，使其運作與開發上有更大程度的提升，但 Typescript 不能在瀏覽器引擎上運作。
Typescript 是 Javascript 的升級版本，最終仍會編譯成 Javascript 提供瀏覽器運作，只是相當程度上增添了許多特性與優點，例如：
#### 在開發早期發現錯誤提示，能早點修復。
{% codeblock lang:JavaScript %}
function addNumbers (num1, num2) {
  return num1 + num2;
}
console.log(addNumbers ('1', '2'))
{% endcodeblock %}

以上的代碼期望的結果是 `1+2 = 3`，但是 JavaScript 本身的隱性型別轉換造成截然不同的結果，得到了 `23`。