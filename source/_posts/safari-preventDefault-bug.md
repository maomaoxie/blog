---
title: Safari IOS 11.3 後 preventDefault 失效問題
date: 2022-08-18 09:12:34
tags:
- safari
- javaScript
- preventDefault
- ios
- mobile
categories:
---
{% img https://i.gifer.com/2BC.gif 300 200 safari-preventDefault-bug %}
沒錯的，我們可愛的 Safari 又又又出現了 bug，來看看這個高級 IE 的除錯日常（Safari 愛好者不要鞭我）

# click 事件綁定
網站開發過程的需求是，當元件沒有 props 連結路徑時就預防 a 連結的預設跳轉行為，以下是元件的 code：
{% codeblock component lang:vue %}
    // HTML
    <a class="w-100 h-100" :class="[ url === '' ? 'pointer-events-none' : 'url-active' ]" @click="changeTab()">
    </a>
    // Script
    <script>
    @Component
    export default class ItemCard extends Vue {
        @Prop({ default: '' }) url?: string;
        private changeTab (event: any) {
            if (this.url === '') {
            event.preventDefault();
            }
        }
    }
    </script>
{% endcodeblock %}

# contextmenu 事件綁定
後來發現 Safari 的瀏覽器，手機與電腦綁定 a 標籤 click 事件都無法達到避免預設行為的效果，於是求救谷歌大神：
{% blockquote %}
To target right click events, use contextmenu rather than mousedown.

`document.querySelector("#GL-Surface").addEventListener("contextmenu",
    function(e) {
        e.preventDefault();
    });
`
Note that the options that appear on right click do appear only when the right click button is released, so I don't think mousedown is at all suitable here.

[Javascript preventDefault() not working in Safari](https://stackoverflow.com/questions/62245019/javascript-preventdefault-not-working-in-safari)
{% endblockquote %}

這裡面提及的**contextmenu**事件是右鍵出現的選單，抱持著嘗試看看的心情監聽contextmenu事件，並綁定了一樣的函式．竟然成功了？？？

第一次看到這個**contextmenu**事件，來瞧瞧 MDN 怎麼說：
{% blockquote %}
    被触发的 contextmenu 事件的默认行为被 preventDefault() 取消了，因此，在第一段右击鼠标时什么也不会发生
    [contextmenu](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/contextmenu)
{% endblockquote %}

# ios11.3 版本後
有人提及**ios11.3 and later**預設行為就失效了
{% blockquote %}
This is not an issue by React and is caused by the recent movement of browser vendors to improve performance on mobile devices.
Some events (usually those that would fire when you scroll) are **getting passive by default**. 
[e.preventDefault() does not work with safari in ios11.3 and later #13369](https://github.com/facebook/react/issues/13369)
{% endblockquote %}

# 改動是為了優化體驗
以上的說明原由來自於性能優化的考量，由於手機裝置在滾動時觸發太多主動事件會嚴重影響體驗，因故某些事件都被調整預設為**被動的（passive）**，來看看以下說明：
（附上不負責任翻譯）

{% blockquote %}
We know that scrolling responsiveness is critical to the user's engagement with a website on mobile, yet touch event listeners often cause serious scrolling performance problems.
在手機裝置中滾動事件的敏感度放大了使用者的體驗，可能造成延遲或性能不好。

Chrome has been addressing this by allowing touch event listeners to be passive (passing the {passive: true} option to addEventListener()) and shipping the pointer events API. These are great features to drive new content into models that don't block scrolling, but developers sometimes find them hard to understand and adopt.
Chrome 在事件監聽上預設了被動參數為 true，避免滾動事件被阻擋。

Developer-defined "passive event listeners" solve this. When you add a touch event with a {passive: true} object as the third parameter in your event handler then you are telling the browser that the touchstart listener will not call preventDefault() and the browser can safely perform the scroll without blocking on the listener.
當你設定事件監聽被動參數為 true，該事件就不會觸發**preventDefault()**避免滾動事件卡卡的

[Making touch scrolling fast by default](https://developer.chrome.com/blog/scrolling-intervention/)
{% endblockquote %}

設定的方式與參數例如這樣：
`window.addEventListener("touchstart", func, {passive: true} );`

# 弔詭的地方
1. 以上說明是針對 Chrome 瀏覽器的性能優化改動，不是 safari 啊？
2. MDN 提及 contextmenu 事件已經不為瀏覽器所支援，但 safari 卻必須使用 contextmenu 事件綁定並且搭配 preventDefault 才能生效？而且註明 safari ios 是唯一不支援此事件的瀏覽器？

{% img /images/safari-preventDefault-bug/0.png 800 200 safari-preventDefault-bug %}
{% img https://c.tenor.com/sAMt7DszgXMAAAAC/%E9%BB%91%E4%BA%BA%E5%95%8F%E8%99%9F.gif 350 200 safari-preventDefault-bug %}

也許下次可以試試看直接把監聽器的 passive 設為 false 試試看 XD

