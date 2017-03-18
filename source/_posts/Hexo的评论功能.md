---
title: Hexo的评论功能
date: 2016-10-06 13:58:51
tags: Hexo
---
换了Litten的yilias主题，简洁、舒服，深得我心。然后在多说创建了站点为博客设置评论的功能。多说能够使用户方便地在后台管理留言，嗯，看起来还是很好用的（如果留言人数多的话）

复制多说提供的代码，打开themes/yilias/layout/_partial/下的文件article.ejs，把最后一段:

```
<% if (!index && post.comments && config.duoshuo_shortname){ %>
  <section id="comments">
  </section>
<% } %>
```

替换成
<!-- more -->
```
<% if (!index && post.comments && config.duoshuo_shortname){ %>
  <section id="comments">
    <!-- 多说评论框 start -->
    <div class="ds-thread" data-thread-key="<%= post.layout %>-<%= post.slug %>" data-title="<%= post.title %>" data-url="<%= page.permalink %>"></div>
    <!-- 多说评论框 end -->
    <!-- 多说公共JS代码 start (一个网页只需插入一次) -->
    <script type="text/javascript">
    var duoshuoQuery = {short_name:"famousczm"};
      (function() {
        var ds = document.createElement('script');
        ds.type = 'text/javascript';ds.async = true;
        ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
        ds.charset = 'UTF-8';
        (document.getElementsByTagName('head')[0] 
         || document.getElementsByTagName('body')[0]).appendChild(ds);
      })();
      </script>
    <!-- 多说公共JS代码 end -->
  </section>
<% } %>
```
**注意：short_name: 后面要填自己站点的short_name，即在创建站点时的站点名称**

保存后，打开站点配置文件_config.yml，在最后一行写上：

```
duoshuo_shortname: famousczm（自己的short_name）
```

完成这些后，我发现博客虽然已经有了评论功能，但是不单单每篇文章下有评论，主页上用来预览的文章下也堆满评论，这很傻啊！找了很久原因，才发现是主题配置文件_config.yml忘记设置了，只要把duoshuo改成这样就好了

```
duoshuo: "famousczm"
```