---
title: Hexo RSS 自動產生器
date: 2022-08-05 15:39:34
tags:
- rss
- hexo
categories:
---

RSS 訂閱功能越來越夯，可以即時通知訂閱戶更新的文章內容，增加回流的流量與網站的熱度，chrome 也有提供 RSS reader 的外掛功能直接讀取 RSS 檔案。
既然建立好了自己的部落格怎麼可以沒有 RSS XD?

先前在專案包嘗試使用 hexo-generator-feed 套件，但是建立出來的 atom.xml 檔案會出現亂碼，後來找到一個套件甚至支援 json 與 rss 格式：

# 安裝 Hexo seed 套件
{% codeblock lang:JavaScript %}
    npm install --save hexo-feed
{% endcodeblock %}

### 檔案格式
hexo-feed 可以設定三種文件格式：atom、json 還有 rss，建立的模板可以透過專案包根目錄的 _config.yml 去設定模板的 path（template）：
{% codeblock _config.yml lang:JavaScript %}
    feed:
        limit: 20
        order_by: "-date"
        tag_dir: "tag"
        category_dir: "category"
        rss:
            enable: true
            template: "themes/layout/template/rss.ejs" 
            output: "rss.xml"
        atom:
            enable: true
            template: "themes/layout/template/atom.ejs"
            output: "atom.xml"
        jsonFeed:
            enable: true
            template: "themes/layout/template/json.ejs"
            output: "feed.json"
{% endcodeblock %}

### 模板內容
{% codeblock atom lang:JavaScript %}
    <?xml version="1.0"?>
    <feed xmlns="http://www.w3.org/2005/Atom">
        <id><%= config.url %></id>
        <title><%= config.title %><%= tag ? ` • Posts by "${tag}" tag` : '' %><%= category ? ` • Posts by "${category}" category` : '' %></title>
        <link href="<%= config.url %>" />
        <updated><%= moment(lastBuildDate).toISOString() %></updated>
        <%_ for (const { name } of (tags || [])) { _%>
        <category term="<%= name %>" />
        <%_ } _%>
        <%_ for (const post of posts) { _%>
        <entry>
            <id><%= post.permalink %></id>
            <title><%= post.title %></title>
            <link rel="alternate" href="<%= post.permalink %>"/>
            <content type="html"><%= post.content %></content>
            <%_ for (const { name } of (post.tags ? post.tags.toArray() : [])) { _%>
            <category term="<%= name %>" />
            <%_ } _%>
            <updated><%= moment(post.date).toISOString() %></updated>
        </entry>
        <%_ } _%>
    </feed>
{% endcodeblock %}