---
title: Js 物件陣列轉換的方法，真香！
date: 2022-08-17 10:22:19
tags:
categories:
---

{% img https://exp-picture.cdn.bcebos.com/3201a8f39187031c063126496a86242fa972ec4a.jpg?x-bce-process=image%2Fresize%2Cm_lfit%2Cw_500%2Climit_1%2Fformat%2Cf_auto%2Fquality%2Cq_80 300 200 真香 %}
[圖片來源](https://exp-picture.cdn.bcebos.com/3201a8f39187031c063126496a86242fa972ec4a.jpg?x-bce-process=image%2Fresize%2Cm_lfit%2Cw_500%2Climit_1%2Fformat%2Cf_auto%2Fquality%2Cq_80)

今天來介紹一下資料處理的好兄弟，`Object.entries` 以及 `Object.fromEntries`。

工作上的需求情境是這樣：
一個物件裡有多個 key，但我只需要複製部份的 key 資料，不用全部時可以使用以下作法。

# Object.entries
工作上的老朋友，常用在物件轉換成陣列時使用，方法特色是將物件內的 key 與 value 轉變成一個**成對的陣列**。
{% codeblock Object.entries lang:JavaScript %}
    const obj = {
    fruits: 'apple',
    beverage: 'choco milk'
    };

    const outcome = Object.entries(obj);
    console.log(outcome);
    // [
        [ 'fruits', 'apple' ],
        [ 'beverage', 'choco milk' ],
    ]
{% endcodeblock %}

# Object.fromEntries
跟上面的相反，常用在陣列轉換成物件時使用，方法特色是將成對的 key 與 value 轉變成一個**物件**。
{% codeblock Object.fromEntries lang:JavaScript %}
    const obj = {
    fruits: 'apple',
    beverage: 'choco milk'
    };

    const entries = Object.entries(obj);
    console.log('entries', entries);

    const fromEntries = Object.fromEntries(entries);
    console.log('fromEntries', fromEntries);
{% endcodeblock %}

# 綜合應用
回到主題，若只需要複製部份的 key 資料可以結合 filter 過濾需要的 key 值：
{% codeblock lang:JavaScript %}
    const obj = {
    fruits: 'apple',
    beverage: 'choco milk'
    };

    const onlyBeverage = Object.entries(obj).filter(([key, value]) => key === 'beverage');
    console.log(onlyBeverage);
    // ['beverage', 'choco milk']

    const transferArrayToObject = Object.fromEntries(onlyBeverage);
    console.log(transferArrayToObject);
    // { 'beverage': 'choco milk' }
{% endcodeblock %}

參考資料[在這](https://masteringjs.io/tutorials/fundamentals/filter-key)