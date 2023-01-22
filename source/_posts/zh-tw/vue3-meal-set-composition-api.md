---
title: vue3 全家桶 - 理解組件化（composition API）
date: 2022-11-13 13:22:31
tags:
- vue3
- compositionApi
- setup
categories:
---

# compostion 的意義
先講結論，composition API 是為了最大程度複用程式碼而誕生的資料結構寫法，
composition 顧名思義組件化就是要自由的拆分然後根據需求重新模組化複用。

之前的[這篇文章](/2022/02/16/zh-tw/vue3-composition-api/)有簡約提到 vue 3 變革的初探，本篇來延伸說明一下 composition API 背後的輪廓，
概念是根據 udemy **Vue 3.x全家桶完全指南与实战**課程：

# options API v.s. composition API
## options API
選項 API 是 vue 既有的一種模組資料分類方式，透過 `export default` 將資料模組化黏著至 vue 實體，
結構如下：
1. data - 一般資料變數
2. computed - 計算屬性資料
3. methods - 函式方法
4. watch - 監聽資料變化

{% codeblock options API lang:JavaScript %}
export default {
  data () {
    ...
  },
  computed: {
    ...
  },
  methods: {
    ...
  },
  watch: {
    ...
  }
}
{% endcodeblock %}

## composition API
組件 API 顧名思義將模組的資料「組件化」，並且背後的命名空間（name space）區分同名的資料與函式，
使同一部件內的資料可以用 `setup` 方法統合整理在一起，寫法範例：

{% codeblock composition API lang:JavaScript %}
export default {
  setup () {
    const data = ref(0);
    const computedData = computed(() => ...);
    function getToday () {
      ...
    }
    watch(data, (newVal, oldVal) => {
      console.log(data.value, newVal, oldVal);
    })
  }
}
{% endcodeblock %}

### 與 options API 共存
composition API 運用命名空間分化了模組各自的片段功能與資料，並且可與 options API 共存

### 外部獨立後匯入 Vue 實體
統整功能與頁面上可以將部分的功能與資料獨立成一個檔案後，匯入模組中：

{% codeblock module A lang:JavaScript %}
  import { ref, computed } from 'vue';
  function moduleFnA () {
    const data = ref(0);
    const computedData = computed(() => ...);
    function getToday () {
      ...
    }
    return data; // 返回值可以為 vue 命名空間區分變數
  }

  export default moduleFnA;
{% endcodeblock %}

{% codeblock moduleB lang:JavaScript %}
  import { ref, computed } from 'vue';
  function moduleFnB () {
    const data = ref(0);
    const computedData = computed(() => ...);
    function getToday () {
      ...
    }
    return data; // 返回值可以為 vue 命名空間區分變數
  }

  export default moduleFnB;
{% endcodeblock %}

{% codeblock importToVue lang:JavaScript %}
  import { ref, computed } from 'vue';
  import moduleFnA from './moduleFnA.js';
  import moduleFnB from './moduleFnB.js';

  export default {
    setup () {
      ... 開始使用 moduleFnA, moduleFnB
    }
  }
{% endcodeblock %}

# Mixins 缺陷
### 配置分散
同一功能分散在不同配置項中（data, computed, method ...），也無法得知各自更新的內容；

### 變數名稱沒有區分
由於命名的作用域相同，同名屬性會互相覆蓋。
#### composition return
composition API 的返回值可以為命名空間區分變數名，為 vue 動態的根據模組添加。