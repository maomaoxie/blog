---
title: Vue 3 的變革
date: 2022-02-16 15:12:05
tags: 
- vue
- vue3
- compositionAPI
categories: 
- vue
- vue3
---
{% img /images/vue3-composition-api/001.png 800 200 vue3-composition-api %}
本篇尚未完整，持續更新中...

你還在用 Vue 2 整理那些物件包裹起來的 data、方法與生命週期的鉤子嗎？還在頭痛父子元件之間傳遞資料的複雜寫法嗎？如果想尋找更接近 typescript 易於維護的團隊開發方式，可以考慮無痛升級 Vue 3 唷！特快車來了還不跟上！


Vue 3 用法隨著版本更新越來越精進與簡潔，尤其是使用漸進式 API 的寫法大大提升了整 code 的整潔與好維護度，以下是一些常用到的整理：
<!-- - Composition API setup() 
- ref()  v.s. reactive() 
- 減少 this 指向，改用 value 取得實體內的data
- 自訂的 css 變數 var() 
- Teleport 
- Fragments 省略根元素 div 
- Emits 獨立出來 
- #fallback 顯示 loading 的內容 
- ::v-slotted 定義插槽樣式 ::v-global 破除scoped的全域樣式
- 生命週期 createApp
- proxy -->

# Composition API
Vue3 一項顯著的變革就是 Composition API，提升撰寫每一個元件專屬方法與資料的便利性：
>Composition API is a set of APIs that allows us to author Vue components using imported functions instead of declaring options. It is an umbrella term that covers the following APIs.
Composition API 使得元件得以直接使用 import 進來的 functions，而不是透過選項來宣告，該特性封裝了以下的 API方法：
### Reactivity API
  例如 `ref()` 和 `reactive()` 可創造響應式（reactive）、計算式（computed）與監聽（watchers）的資料狀態
### Lifecycle Hooks
  例如 `onMounted()` 和 `onUnmounted()`, 容許使用編成方式撰寫元件的生命週期鉤子
### Dependency Injection
  例如 `provide()` and `inject()`, 提供一個`祖父 -> 父(跳過) -> 子`元件之間依賴注入（Dependency Injection）的接口，可以更好的分化元件之間的需求，了解更多內容可以詳讀[這篇](https://penueling.com/%E6%8A%80%E8%A1%93%E7%AD%86%E8%A8%98/vue3-%E7%9A%84%E8%B3%87%E6%96%99%E7%8B%80%E6%85%8B%E7%AE%A1%E7%90%86%EF%BC%8Cprovide-inject%E3%80%81vuex/)

  簡單下個速記法：
  provide() 與 inject() 看做是 prop 的昇級版，看個示意圖吧！
  {% img /images/vue3-composition-api/002.png 800 200 vue3 composition api %}
### Setup script
Vue 3 將 Composition API 整合在單個元件的 `<script setup>` 標籤中，可以直接默認寫在裡面的內容都是提升到 `created()` 和 `beforeCreated()`的生命週期階段去實踐的
>In Vue 3, it is also primarily used together with the `<script setup>` syntax in Single-File Components.
{% codeblock component.vue lang:JavaScript  %}
<script setup>
  import { ref, onMounted } from 'vue'

  // reactive state
  const count = ref(0)

  // functions that mutate state and trigger updates
  function increment() {
    count.value++
  }

  // lifecycle hooks
  onMounted(() => {
    console.log(`The initial count is ${count.value}.`)
  })
  </script>

  <template>
    <button @click="increment">Count is: {{ count }}</button>
  </template>
{% endcodeblock %}
____

# Reactivity API
多了一層代理（proxy）包裹所有的資料（data），使得 vue3 在處理 data 上有些變革：
- ref() 傳入 primative value 
- reactive() 傳入 object value
該選擇哪一種可以參考這篇 [ref vs reactive in Vue 3?](https://stackoverflow.com/questions/61452458/ref-vs-reactive-in-vue-3)，滿有助益的解釋非常通透呢！

### ref ()
當你需要將資料型態重新賦值（reassign）時可以選用 `ref()`，其實該方法背後也調用了 `reactive()` 只是你不知道而已，原始型別非常適合使用此代理包裹，另外當你的物件型別資料並沒有一開始的屬性值，例如空物件或空陣列，也可以考慮使用這個方式。
需要特別注意的點就是該方法取值要添加 `.value` 才能取得。
{% blockquote %}
ref() Use-Case
You'll always use ref() for primitives, but ref() is good for objects that need to be reassigned, like an array.
{% endblockquote %}
{% blockquote %}
When you write ref([]) it is equivalent to ref(reactive([])).
{% endblockquote %}

### reactive ()
當需要將物件型別的資料型態其中某個屬性值透過指向參考（dot notation）來修改值的時候可以選用 `reactive()`，前提在於你知道該物件內容的屬性值有哪些，屬性值彼此之間的牽動關聯性是甚麼，甚至可以直接撰寫 `computed()` 來決定某屬性值，該方法須注意不能被重新賦值更改指向參考，完全不會理你。
{% blockquote %}
看到透過 reactive 包裝後的資料連 computed 都包裝了進去，是不是覺得非常的方便，不需要再另外切開去寫computed ，在尋找相依性的資料也變簡單。
https://medium.com/i-am-mike/vue-3-ref-%E8%B7%9F-reactive-%E6%88%91%E8%A9%B2%E6%80%8E%E9%BA%BC%E9%81%B8-2fb6b6735a3c
{% endblockquote %}
{% blockquote %}
reactive() Use-Case
A good use-case for reactive() is a group of primitives that belong together:
{% endblockquote %}

# props
### 非必要的 props 
只需要在非必要的 props 加上`?` 就可以囉！不然 typescript 會一直跳出警語 `ts(2322)`
{% codeblock %}
const props = defineProps<{ 
  span: number, 
  label: string, 
  name: string, 
  placeholder: string, 
  prefix?: {
    type: string,
    default: ''
  }, 
  suffix?: {
    type: string,
    default: ''
  }, 
}>()
{% endcodeblock  %}



# 方法
### toRaw
轉換透過vue創造的代理物件為普通物件，可轉換的代理物件有 `reactive()`, `readonly()`, `shallowReactive()` 或 `shallowReadonly()`。
This is an escape hatch that can be used to temporarily read 
{% blockquote %}
Returns the raw, original object of a Vue-created proxy.
{% endblockquote %}