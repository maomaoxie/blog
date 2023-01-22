---
title: vue3 全家桶 - ref 與 reactive 區別
date: 2022-11-13 15:00:28
tags:
- vue3
- ref
- reactive
categories:
---

# 響應式數據
配合新的 setup 函式，偵測響應式數據的方法有 ref 與 reactive，響應式數據就是在數據變化時可以敏銳偵測到，並且隨之變動 virtual dom，達到 MVVM 的資料締結。

# ref 函式
## 取代 data 函式，返回一個代理對象
ref 接受所有型別的數據，並且將資料包裝成一個代理，需要透過 `.value` 取得原始資料的數據，但須注意的是代理取得的值並非原始數據：
{% codeblock ref lang:JavaScript %}
let str = '字串';
let refStr = ref(str);
console.log( str === refStr.value ); // false
console.log( refStr ); // 返回代理對象
{% endcodeblock %}

## 模板訪問 ref 無須透過 `.value`
在 html template 中代理數據可以直接使用變數名稱取得：
{% codeblock ref lang:JavaScript %}
<template>
  ...
  <h1>My favorite food is {{ apple }}.</h1>
</template>

<script>
  import { ref } from 'vue';
  export default {
    setup () {
      const apple = ref('apple');
      return { apple };
    }
  }
</script>
{% endcodeblock %}

# reactive 函式
## 只接受 object & array 形式的資料
若給予 reactive 原始型別的數據，會噴錯誤訊息。

## 監聽深層資料變化
物件內部資料變化 ref 無法偵測到，不會觸發 watch 乃至重新渲染 virtual dom：
