---
title: 建構子方法與實例化方法
date: 2022-05-14 21:37:03
tags:
- javascript
- constructor
- instance
- methods
categories: javascript
---
{% img /images/constructor-vs-instance/0.png 800 200 constructor-vs-instance %}

在某天好奇想了解 Vue 3 的 defineProperty 原理搜尋了 `Object.defineProperty()` 這個方法時，看見以下說明：

{% blockquote MDN https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty Object.defineProperty() %}
靜態方法 `Object.defineProperty()` 會直接對一個物件定義、或是修改現有的屬性。執行後會回傳定義完的物件。
備註：這個方法會直接針對 Object 呼叫建構子（constructor），而不是 Object 型別的實例。
{% endblockquote %}
其中的**直接針對 Object 呼叫建構子（constructor），而不是 Object 型別的實例**這句話突然讓我驚醒了，以前一直不能理解 javascript 中呼叫原生方法時，為何會有以下的區別：

## 透過建構器呼叫
`Object.methods(objInstance)` -> 例如 Object.keys(someObj)
這裡的 Object 是建構函式本身，未實例的藍圖（constructor）。

## 透過實例呼叫
`objInstance.methods(parameters)` -> 例如 someObj.hasOwnProperty('prop')
這裡的 Object 是實例化的物件（instance）。

## Constructor Static Methods
#### 構造器 靜態方法
以下的例子是呼叫 Object 建構子中的原生 keys 方法，而不需要 new 一個物件實例就可以使用，靜態方法的特色是無需使用任何建構子中的 this 資料就可以直接使用。
{% codeblock 建構子方法 lang:JavaScript  %}
  const someone = {
    name: 'Adam',
    carrer: 'teacher',
    sex: 'male'
  };
  const dataKeys = Object.keys(someone);
  console.log(dataKeys);
  // ["name","carrer","sex"]
{% endcodeblock %}

### Instance methods
#### 等號賦值 實例化方法
以下則是呼叫 Array 的實例化 push 方法，雖然也不是透過 new 來建立一個陣列，卻也是使用賦值一個陣列來建立陣列的實例，並且使用原生 push 方法。
{% codeblock 實例化方法 - 賦值 lang:JavaScript  %}
  const friends = [ 'Cally', 'Donna', 'Jell' ];
  friends.push('Liang');
  console.log(friends);
  // ["Cally","Donna","Jell","Liang"]
{% endcodeblock %}

#### 建構器 實例化方法
透過 new 來建立一個陣列。
{% codeblock 實例化方法 - 構造器 lang:JavaScript  %}
  const animals = new Array('bunny', 'cat', 'puppy', 'hamster');
  animals.unshift('bird');
  console.log(animals);
  // ["bird","bunny","cat","puppy","hamster"]
{% endcodeblock %}

## 構造器靜態方法補充
#### 無法取得構造器的this資料
{% blockquote %}
- The static method also cannot see the instance variable state so if we try to call the nonstatic method from the static method compiler will complain.
- The static method can be used to create utility functions.
https://www.educba.com/javascript-static-method/
{% endblockquote %}

從上述可以得知靜態方法是不能取用構造器建構子（constructor）內的變數的（this binding），通常會撰寫純函式（pure function）以保持無狀態的特性，如同 Math 方法。
靜態方法適合用來當作全局複用的函式，適合較無副作用的邏輯。

## 兩種方法的原型鍊關係
若展開一個實例化的物件，會發現：
1. 建構器方法存在於建構子物件中（constructor），且只能透過建構器呼叫，例如 Object.assign()；
2. 實例化方法則存在於原型上（prototype），需要實例化之後才能呼叫，例如 objInstance.toLocaleString()