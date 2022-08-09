---
title: 如何在 vuetify 元件中渲染 v-html
date: 2022-08-09 12:09:43
tags:
- vue
- vuetify
- html
categories:
- vuetify
---

{% img /images/vuetify-slot-vhtml/0.png 800 200 vuetify-slot-vhtml %}
使用 vuetify 元件時常有的狀況劇就是 - 標題的 UI 設計需要客製化，例如放上 fontawesome 的 icon，但你的元件是 vuetify 父元件（v-select），客製的對象是迴圈後的子元件（選單下的選項 v-selection），這時候要如何成功渲染 fontawesome 的 icon ?

{% codeblock vuetify lang:JavaScript %}
    <v-select
        :items="items"
        label="Standard"
    ></v-select>
{% endcodeblock %}

# 使用 `<i></i>` 標籤
目前可以知道的小撇步就是 fontawesome 的 icon 要使用 vue v-html 渲染，必須使用 `<i></i>` 標籤才能成功，不能放入 vuetify  `<v-icon></v-icon>`，無法正確編譯成 html。

# 在父元件內使用 v-slot
可以知道目前的元件結構會是這樣的：
v-select（父）
        ｜
    v-selection（子）

選項是這樣：
{% codeblock lang:JavaScript %}
    [
        { value:"0",text: "<i class="fab fa-apple"></i> apple" },
        { value:"1",text: "<i class="fab fa-google-drive"></i> google" },
    ]
{% endcodeblock %}

vuetify 父元件（v-select）透過 props items 選項的陣列可以使用 v-slot 轉換成 html 模板傳入：
{% blockquote %}
    Override the item and selection slots, and use v-html.
    參考網址：https://stackoverflow.com/questions/56665185/show-raw-html-in-vuetify-v-select
{% endblockquote %}

### vue 2 版本
由於子元件會有 default 選項，要使用插槽針對 **selection** 修改；
而下拉選項要使用插槽針對 **item** 修改。

##### 寫法 1
{% codeblock slot 範例 1 lang:JavaScript %}
    <v-select :items='item'>
        <template v-slot:item='{item}'>
            <div v-html='item.text'/>
        </template>
        <template v-slot:selection='{item}'>
            <div v-html='item.text'/>
        </template>
    </v-select>
{% endcodeblock %}

##### 寫法 2
{% codeblock slot 範例 1 lang:JavaScript %}
    <v-select :items='item'>
        <template v-slot:item='{item}'>
            <div v-html='item.text'/>
        </template>
        <template v-slot:selection='{item}'>
            <div v-html='item.text'/>
        </template>
    </v-select>
{% endcodeblock %}


### 簡潔版本
{% codeblock slot 範例 2 lang:JavaScript %}
    <v-select :items='item'>
        <div slot='item' slot-scope='{item}' v-html='item.text'/>
    </v-select>
{% endcodeblock %}

### vue 3 版本
{% codeblock slot 範例 3 lang:JavaScript %}
    <template #item='{item}'>
        <div v-html='item.text' />
    </template>
{% endcodeblock %}

# vuetify api 說明
在每個元件的 api 文件下方都有提供現成的插槽可以使用：
{% img /images/vuetify-slot-vhtml/1.png 800 200 vuetify-slot-vhtml %}