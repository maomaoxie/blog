---
title: Graph QL 新世代 API
date: 2022-02-25 10:48:36
tags:
- api
- graphQL
- facebook
categories:
- graphQL
---
{% img /images/graph-ql-api/0.png 800 200 [css-less [css-less]] %}
隨著行動裝置越來越多元，服務與資料需求越來越錯綜複雜，因應這樣背景的 Graph QL API 就誕生啦！

2012 年 facebook 內部研發出來的新形態 API，2015 年提出發表，特點是前端可以直接進資料庫，不需要透過後端查詢撈取資料。
概念有一點類似用前端語言操作 `MySQL`。前端也擁有更彈性的空間來制定查詢資料的規則與方式，有更大話語權，但相對來講也增加資料設計的難度，在思考的維度上需要更廣更深才能靈活應付多變的資料需求。


# Graph QL 操作概念
{% img /images/graph-ql-api/1.png 800 200 [css-less [css-less]] %}
>使用者透過輸入定義好的語法，取得所需的資料 (如 `SELECT`) 或是修改指定的資料 (如 `CREATE` 、 `UPDATE`)。
## 資料的格式
#### 前端 request 長相 
{% codeblock example lang:JavaScript  %}
  query {
    User(id: 'aefk34kasl9') {
      name
      posts{
        title
      }
      followers(last: 3){
        name
      }
    }
  }
{% endcodeblock %}

#### 資料庫 response 長相
{% codeblock example lang:JavaScript  %}
  {
    "data": {
      "User": {
        "name": "Mawchu"
        "posts":[
          { title: "喵喵的一天" }
        ]
        "followers": [
          { "name": "Cally" },
          { "name": "Donna" },
          { "name": "Jell" },
        ]
      }
    }
  }
{% endcodeblock %}

# 補充
>Schema，香港和中國大陸翻譯為模式或架構，在資料庫系統中是形式語言描述的一種結構，是對象的集合，可包含各種對象如：表、欄位、關係模型、視圖、索引、包、存儲過程、子程序、隊列、觸發器、數據類型、序列、物化視圖、同義詞（synonym）、database link、directory、XML schema等。

參考資料：
{% blockquote %}
  hiskio 課程 API 整合實戰｜RESTful 第三方串接應用
  https://ithelp.ithome.com.tw/articles/10200678
{% endblockquote %}