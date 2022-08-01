---
title: Express 路由模組化
date: 2022-02-20 14:12:53
tags:
- backEnd
- node
- express
- router
- 
categories: node.js
---
{% img /images/express-router-module/1.png 800 200 express-router-module %}
###### 圖片來源: https://inmywordz.com/archives/504
當路由越寫越多變成霍爾的移動城堡一樣雜亂肥大時，自己都看得眼花撩亂啦～考慮使用模組化的概念來統整自己的後端路由是很好的選擇，那麼就來開始吧！

___
# Express.js 4.0 Router 的誕生
Express 輕量框架提供很多整理路由的方式，Express.js 4.0 導入了新的 **Router 功能**，可以更彈性的撰寫子路由規則以及中介軟體啦！
{% blockquote %}
Express.js 4.0 有加入一個新的 Router 功能，它就像一個迷你的應用程式，可以讓應用程式內部的路由撰寫更方便、更有彈性。
Express.js 在 4.0 版中有許多新的功能，其中一項主要的功能就是 Router，以下我們介紹如何使用 Router 功能來撰寫應用程式。
https://blog.gtwang.org/programming/learn-to-use-the-new-router-in-expressjs-4/
{% endblockquote %}
___
# app.use() 產生父路由與路由頭中介
{% blockquote %}
  app.use(); mounts middleware for all routes of the app (or those matching the routes specified if you use app.use('/ANYROUTESHERE', yourMiddleware());).
{% endblockquote %}
app.use() 是啟用一個**路由頭（父路由）**，並且只要符合該路由頭的**每一個子路由**，該中介軟體都會啟用，很適合用在登入後驗證是否有登入的會員，是否有訪問路由頁面的權限，以撰寫一些驗證信息。
___
# router.methods() 產生子路由
#### 舊的子路由寫法
{% codeblock  lang:JavaScript %}
  const express = require('express');
  const app = express();
  const router = express.Router();
  const port = 5500;

  app.get('/', function(req, res) {
    res.send('舊的寫法：歡迎來到首頁')
  })

  app.listen(port, () => {
    console.log(`Example app listening on port ${port}`)
  })
{% endcodeblock %}
{% img /images/express-router-module/2.png 380 200 express-router-module %}

#### 新的子路由寫法
{% codeblock  lang:JavaScript %}
  const express = require('express');
  const app = express();
  const router = express.Router();
  const port = 5500;

  router.get('/', function(req, res) {
    res.send('新的寫法：歡迎來到首頁')
  })
  router.get('/page', function(req, res) {
    res.send('新的寫法：歡迎來到分頁')
  })

  // 制定一個路由頭（父路由）
  app.use('/api', router)

  app.listen(port, () => {
    console.log(`Example app listening on port ${port}`)
  })
{% endcodeblock %}
{% img /images/express-router-module/3.png 380 200 express-router-module %}
{% img /images/express-router-module/4.png 478 235 express-router-module %}

我自己是看不太出來差別，不過文章都是寫道 router 就像一個小型 app 提供你整理路由系統更好的方法，而具體的差異如下：
{% blockquote %}
  "A Router doesn't .listen() for requests on its own". That might be the main difference. 
  It's useful for separating your application into multiple modules -- creating a Router in each that the app can require() and .use() as middleware. 
{% endblockquote %}
  1. Router 不能單個路由做 port 監聽 
  2. Router 可以使用 require() 與 use() 模組化管理子路由
  哪一種比較好眾說紛紜，基本上能適合專案模式的都是好方法。

{% blockquote %}
  router.use(); mounts middleware for the routes served by the specific router
  router.get is only for **defining subpaths**.
  Consider this example:
  {% codeblock  lang:JavaScript %}
    var router = express.Router();
    app.use('/first', router); // Mount the router as middleware at path /first
    router.get('/sud', smaller);
    router.get('/user', bigger);
  {% endcodeblock %}
  If you open /first/sud, then the smaller function will get called.
  If you open /first/user, then the bigger function will get called.
  In short, app.use('/first', router) mounts the middleware at path /first, then router.get sets the subpath accordingly.
  https://stackoverflow.com/questions/27227650/difference-between-app-use-and-router-use-in-express
{% endblockquote %}
___
# router.use() 子路由中介
router.use() 是產生子路由中介（middleware）的方式：
{% colorquote info %}
在使用 middleware 時必須要注意他的放置位置必須要在 routes 之前，程式在執行的時候會依據 middleware 與 routes 的先後順序來執行，如果不小心將 middleware 放在 routes 之後，那麼在 routes 處理完請求之後就會結束處理的流程，這樣 middleware 就根本不會執行。
https://blog.gtwang.org/programming/learn-to-use-the-new-router-in-expressjs-4/
{% endcolorquote  %}
{% codeblock  lang:JavaScript %}
// 中介軟體要寫在 router 上面
router.use(function(req, res, next) {
  // 輸出記錄訊息至終端機
  const url = `${req.protocol}://${req.hostname}:${port}${req.baseUrl}${req.path}`;
  console.log(url, '子路由會經過我');
  // 繼續路由處理
  next();
})
router.get('/', function(req, res) {
  res.send('新的寫法：歡迎來到首頁')
})
router.get('/page', function(req, res) {
  res.send('新的寫法：歡迎來到分頁')
})
{% endcodeblock %}
{% img /images/express-router-module/5.png 478 235 express-router-module %}
___
# router 模組化

#### 子路由模塊
將幾個子路徑依功能切開可以更方便的統整路由：
{% codeblock  lang:JavaScript ./routers/cat.js  %}
  const express = require('express');
  const router = express.Router();
  const port = 5500

  router.use(function(req, res, next) {
      // 輸出記錄訊息至終端機
      const url = `${req.protocol}://${req.hostname}:${port}${req.baseUrl}${req.path}`;
      console.log(url, '喵喵子路由會經過我');
    
      // 繼續路由處理
      next();
  })

  router.get('/', function(req, res) {
      const url = `${req.protocol}://${req.hostname}:${port}${req.baseUrl}${req.path}`;
      res.send('來到喵喵子路由');
  });
  router.get('/meow', function(req, res) {
      res.send('喵喵分頁');
  });

  module.exports = router;
{% endcodeblock %}

#### 引用模塊
在入口引入模塊：
{% codeblock  lang:JavaScript index.js  %}
  const express = require('express');
  const app = express();
  const router = express.Router();
  const catsRouter = require('./routes/cat')
  const port = 5500;

  // 模組寫法
  app.use('/cat', catsRouter) 

  app.listen(port, () => {
    console.log(`Example app listening on port ${port}`)
  })
{% endcodeblock %}
{% img /images/express-router-module/6.png 478 235 express-router-module %}
{% img /images/express-router-module/7.png 478 235 express-router-module %}
{% img /images/express-router-module/8.png 478 235 express-router-module %}
___
參考文章：
{% blockquote %}
https://stackoverflow.com/questions/27227650/difference-between-app-use-and-router-use-in-express
https://blog.gtwang.org/programming/learn-to-use-the-new-router-in-expressjs-4/
http://expressjs.com/zh-tw/api.html#req
{% endblockquote %}