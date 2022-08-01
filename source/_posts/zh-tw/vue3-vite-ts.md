---
title: Vue3 + Vite + Ts 專案建置
date: 2022-02-18 20:16:27
tags:
- vue3
- vite
- typescript
categories: 
- vue
- vue3
---

Vite 問世以後總算不用再看著打包的百分比甚麼時候跑完了（灑花，按下 ctrl S 直接查看效果棒棒的！
不過再也沒有藉口可以喝口拿鐵啦XD

簡單介紹一下 Vite 是甚麼：
{% blockquote %}
 Vite（法语意为 "快速的"，发音 /vit/，发音同 "veet"）是一种**新型前端构建工具**，能够显著提升前端开发体验。它主要由两部分组成：
- 一个开发服务器，它基于 原生 ES 模块 提供了 丰富的内建功能，如速度快到惊人的**模块热更新（HMR）**。
- 一套构建指令，它使用 Rollup 打包你的代码，并且它是预配置的，可输出用于生产环境的高度优化过的静态资源。
{% endblockquote %}
先打預防針，小菜雞本人對於類別的物件導向了解尚淺，ts 的運用也不熟悉，導入使用再一步一步學習，這裡著重於專案是否能啟用。
本篇簡略說明建置 Vue3 + Vite + Ts 的專案過程中使用的方法：
# 快速建置專案
 ``` bash
  # npm 6.x
  npm create vite@latest my-vue-app --template vue
  ```
  ``` bash
  cd my-project
  npm install
  npm run dev
  ```
# 使用 ant design vue UI
  antd 的介面優雅簡約個人滿喜歡的，配色切換的設定也很方便
  - 安裝套件
  ``` bash
  npm i --save ant-design-vue@next
  ```
  - 新建 libs 資料夾在 src 底下，新增 antdv.ts
    {% codeblock lang:javascript antdv.ts %}
      import type { App } from "vue";
      import Antd from "ant-design-vue";
      import "ant-design-vue/dist/antd.less";

      export function setupAntd(app: App<Element>): void {
        app.use(Antd);
      }
    {% endcodeblock %}

  - 全局入口引用 antd，並且註冊 antd icon
    {% codeblock lang:javascript main.ts %}
      import { createApp } from 'vue'
      import App from './App.vue'
      import { setupAntd } from "./libs/antdv"; // 新增++
      import * as Icons from '@ant-design/icons-vue';

      const app = createApp(App);
      const icons :any = Icons;
      setupAntd(app); 
      createApp(App);

      // 父組件全局註冊ICONS
      for(const i in icons) {
        app.component(i, Icons[i]);
      }

      app.mount('#app')
    {% endcodeblock %}
  - 在 vite.config.ts 修改主要配色 primary 變數，這裡踩了很多坑，處理器的設定物件結構一直不對（哭
  - 由於 antd 使用的 css 預處理器是不熟悉的 less，無奈一個專案還是選擇一個預處理器就好，只能新學一個寫法來制定自己的全局 css 囉！scss 先放一邊啦。套用全局 css 只需在 additionalData 裡面放置路徑的字串。 
    {% codeblock lang:javascript vite.config.ts %}
    import { defineConfig } from 'vite'
    import vue from '@vitejs/plugin-vue'

    // https://vitejs.dev/config/
    export default defineConfig({
      plugins: [vue()],
      css: {
        preprocessorOptions: {
          less: {
            javascriptEnabled: true,
            modifyVars: {
              'primary-color': '#94D0C9',
              'link-color': 'red',
            },
            additionalData: '@import "./src/assets/style/global.less";',
          }
        },
      },
    })
    {% endcodeblock %}

# vuex4 導入與建置 mapStates 模組
  - 安裝 vuex4
  ``` bash
  npm install vuex@next
  ```
  - 全局入口引用 vuex，多了一把**鑰匙（key）**需要設置並且傳入 **useStore()** 中才能啟用 vuex，鑰匙的型態是 symbol()
    {% codeblock lang:javascript main.ts %}
      import { createApp } from 'vue'
      import App from './App.vue'
      import { setupAntd } from "./libs/antdv"
      import * as Icons from '@ant-design/icons-vue'
      import { store, key } from './store/store' // 新增++

      const app = createApp(App);
      const icons :any = Icons;
      app.use(store, key) // 新增++
      setupAntd(app); 
      createApp(App);

      // 父組件全局註冊ICONS
      for(const i in icons) {
        app.component(i, Icons[i]);
      }

      app.mount('#app')
    {% endcodeblock %}
  - 撰寫 vuex 配置檔案，為了方便後續使用模組導入 mapStates 的方式，統一放在 src 底下的資料夾 store 內：
    {% img /images/vue3-vite-ts/01.png 800 200 [Vue3 [Vue3]] %}
  - 與 vuex3寫法上的不同點：
    以前使用 new Store()，vuex4改用 createStore() 建立倉庫。
    {% codeblock lang:javascript store.ts %}
      import { InjectionKey } from 'vue'
      import { createStore, Store } from 'vuex'
      import axios from 'axios'
      import ApiClient from '../ApiClient'

      // 为 store state 声明类型
      export interface State {
        count: number
      }

      // 定义 injection key
      export const key: InjectionKey<Store<State>> = Symbol()

      export const store = createStore<State>({
        state: {
          count: 0
        },
        getters: {
        },
        mutations: {
        },
        actions: {
        },
      })
    {% endcodeblock %}
    
  - 在 Vue3.x 版本中使用 vuex4 需要透過 useStore() 方法才能訪問 store 內容：
    {% codeblock lang:javascript store.ts %}
      
    {% endcodeblock %}