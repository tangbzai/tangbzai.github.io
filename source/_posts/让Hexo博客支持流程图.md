---
title: 让Hexo博客支持流程图
date: 2020-03-22 19:10:54
tags: 记录
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