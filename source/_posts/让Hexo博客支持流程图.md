---
title: 让Hexo博客支持流程图
date: 2020-03-22 19:10:54
tags: 
  - 记录
  - 教程
---

本文讲的是hexo的插件[hexo-filter-mermaid-diagrams](https://github.com/webappdevelp/hexo-filter-mermaid-diagrams)
## hexo-filter-mermaid-diagrams

### 1. 快速使用
1. 安装<br>
在博客的根目录下使用命令
```
$ yarn add hexo-filter-mermaid-diagrams
或者
$ npm install hexo-filter-mermaid-diagrams
```
2. 配置<br>
配置hexo的配置文件`_config.yml`

```
# mermaid chart
mermaid: ## mermaid url https://github.com/knsv/mermaid
  enable: true  # default true
  version: "7.1.2" # default v7.1.2
  options:  # find more api options from https://github.com/knsv/mermaid/blob/master/src/mermaidAPI.js
    #startOnload: true  // default true
```

3. 在模版文件引入mermaid.js
在`/themes/`里找到自己正在使用的主题的文件夹 ,打开里面的`layout`文件夹，在里面找到一个在文章页面有被加载的模版文件(官方写的是`after_footer`文件，我的主题没有)根据下面情况引入相应内容。

- 我的主题使用的是[fluid](https://github.com/fluid-dev/hexo-theme-fluid)就添加下面的代码到`/themes/fluid/layout/plugins/footer.ejs`, 如果是其他尾缀类型的模版例如`pug`或`swig`请去看[官方文档](https://github.com/webappdevelp/hexo-filter-mermaid-diagrams)<br>

```
<% if (theme.mermaid.enable) { %>
  <script src='https://unpkg.com/mermaid@<%= theme.mermaid.version %>/dist/mermaid.min.js'></script>
  <script>
    if (window.mermaid) {
      mermaid.initialize({theme: 'forest'});
    }
  </script>
<% } %>
```

添加完成后，回到博客根目录的_config.yml，把external_link的值改为false，默认的为true

### 2. 出现问题
1. 没有效果<br>
*记得`hexo clean`<br>
原因：语法不一样,三个点后面填写`mermaid`

```
` ` `mermaid
内容
` ` `
```

2. 报错Error: \<svg> attribute viewBox: Expected number, "0 0 -Infinity -Infin…".
原因：主题的代码高亮样式冲突了。（主题版本为v1.7.4）
将主题的代码高亮关了就好了.
```
highlight: # 代码高亮
  enable: false
```

### 优化
在模版`footer`文件引入mermaid.js的话会在博客的每个页面都加载这个文件，而我们往往并不是每个页面都会有用到这个插件。
1. 把==配置部分==放到==主题配置==(`/themes/fluid/_config.yml`)里<br>
```
mermaid: # 流程图
    enable: true
    specific: true  # 开启后，只有在文章 Front-matter 里指定 `mermaid: true` 才会在文章页启动公式转换，以便在页面不包含公式时提高加载速度
```
（就快速使用的第二步里的内容多了`specific`来控制是否启动转换）
*如果不想放在主题配置里把第二步的`theme.post`改成`config`就好了
2. 在`/themes/fluid/layout/_partial/plugins`新建`mermaid.ejs`文件，内容如下<br>
```
<% if(theme.post.mermaid.enable && (!theme.post.mermaid.specific || (theme.post.mermaid.specific && page.mermaid))) { %>
<%- js_ex(theme.static_prefix.mermaid, "mermaid.min.js") %>
<script>
    if (window.mermaid) {
        mermaid.initialize({ theme: 'default' });
    }
</script>
<% } %>
```
3. 引入文件到`scripts.ejs`<br>
在`/themes/fluid/layout/_partial/scripts.ejs`最下面添加
```
<%- partial('_partial/plugins/mermaid.ejs') %>
```
4. 最后在`_static_prefix.yml`添加cdn即可
在`/themes/fluid/source/_static_prefix.yml`最下面添加
```
mermaid: https://cdn.bootcss.com/mermaid/8.4.8/
```
*此处写死了版本号，想移到配置文件中请自行修改

### 总结
优化部分的代码还是蛮好懂的，照着本身自带的math进行修改即可。就是代码高亮部分导致的报错并无法显示耗费了许多时间。下次遇到问题先在github的Issues里看看。