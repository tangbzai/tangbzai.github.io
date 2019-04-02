---
title: 搭建Hexo博客
date: 2019-03-29 00:30:03
tags:
    - 记录
---
# Hexo 博客搭建
## 1.安装git
1.	windows：到git官网上下载 [Download git](https://www.git-scm.com/download/win/),下载后会有一个Git Bash的命令行工具，以后就用这个工具来使用git。
2. linux：一行代码
```
sudo apt-get install git
```

## 2.安装nodeJS
Hexo是基于nodeJS编写的，所以需要安装一下nodeJs和里面的npm工具。
- windows：nodejs选择LTS版本就行了。(LTS:长期支持版本)
- linux：
```
	sudo apt-get install nodejs
	sudo apt-get install npm
```
2.	安装完后，打开命令行
```
    node -v
	npm -v
```
## 3.安装Hexo
1.先创建一个文件夹blog，然后cd到这个文件夹下（或 文件夹右键->git bash）
```
    npm install -g hexo-cli
```
- 可用`hexo -v`查看一下版本

初始化hexo
```
    hexo init myblog
    cd myblog //进入文件夹内
    npm install
```
新建完成后，指定文件夹目录下有：
- node+modules: 依赖包
- public: 存放生成的页面
- scaffolds: 生成文章的一些模板
- source: 存放文章
- themes: 主题
- _config.yml: 博客的配置文件

生成静态文件
<br>启动服务器 `http://localhost:4000`
```
    hexo g
    hexo sever  
```
## 4.注册Github账号、注册Github
1. 注册Github帐号
2. 登陆后需选择New repository,新建仓库
3. 创建一个与用户名相同的仓库，后面加.github.io
4. 点击create repository。

## 5.生成SSH添加到GitHub
1. 设置git的身份信息（名字与邮箱）
```
    git config --global user.name "name"
    git config --global user.email "email"
```
2. 创建SSH
```
ssh-keygen -t rsa -C "email"
```
3. 在 系统盘->用户->（用户名）->.ssh 目录下找到.ssh文件
>ssh，简单来讲，就是一个秘钥，其中，`id_rsa`是你这台电脑的私人秘钥，不能给别人看的，`id_rsa.pub`是公共秘钥，可以随便给别人看。把这个公钥放在GitHub上，这样当你链接GitHub自己的账户时，它就会根据公钥匹配你的私钥，当能够相互匹配时，才能够顺利的通过git上传你的文件到GitHub上。
>
>作者：zjufangzh<br>
>来源：CSDN <br>
>原文：https://blog.csdn.net/sinat_37781304/article/details/82729029 <br>
>版权声明：本文为博主原创文章，转载请附上博文链接！

4. 在GitHub的setting中，找到SSH keys的设置选项，点击`New SSH key`
把你的`id_rsa.pub`里面的信息复制进去。
5. 查看是否成功
```
   ssh -T git@github.com 
```
## 6.将Hexo部署到GitHub
1. 将hexo和GitHub关联起来,打开站点配置文件 _config.yml，翻到最后，修改为
```
deploy:
  type: git
  repo: https://github.com/YourgithubName/YourgithubName.github.io.git
  branch: master
```
YourgithubName就是你的GitHub账户
2. 安装 **deploy-git**
```
npm install hexo-deployer-git --save
```
3. 使用命令部署
```
hexo clean
hexo generate
hexo deploy
```
>`hexo clean`清除了你之前生成的东西，也可以不加。<br>
`hexo generate` 顾名思义，生成静态文章，可以用 `hexo g`缩写<br>
`hexo deploy` 部署文章，可以用`hexo d`缩写

过一会儿就可以在`http://yourname.github.io` 这个网站看到你的博客了
## 7.设置个人域名
这步之前博客的地址是`yourname.github.io`,如果想换个就买个域名。
买好了就在域名控制台，选择域名点**解析**添加192.30.252.153 和 192.30.252.154 是GitHub的服务器地址。
**解析线路选默认**
1. 登陆GitHub, 进入之前创建的仓库，点击settings, 设置Custiom domain,输入你的域名<br>
2. `source`创建一个名为CHANME的文件，不要后缀，写上域名。
3. 最后在**git bash** 中输入
```
hexo clean
hexo g
hexo d
```
## 8.使用腾讯开发者平台的pages服务进行分流
注：现在coding不能使用pages,得升级账号，腾讯开发者平台的才有此功能。
1. 注册腾讯云开发者平台账号，或coding用户升级。
2. 新建项目
3. 使用Git上传文件项目里
4. 添加ssh公钥到个人账号的公钥上
5. 选择项目->代码->pages服务，按照指引完成即可
6. 修改配置文件
```
deploy:
  type: git
  repository:
      github: git@github.com:ShomyLiu/ShomyLiu.github.io.git
      coding: git@git.coding.net:shomyliu/shomyliu.git
  branch: master
 ```
>在博客的source/目录下需要创建一个空白文件,至于原因，是因为 coding.net需要这个文件来作为以静态文件部署的标志。就是说看到这个Staticfile就知道按照静态文件来发布。<br>
coding还需要在根目录下建一个.nojekyll空文件（coding page 配置hexo报错 the xx theme not be found<br>
在根目录下建一个.nojekyll空文件就可以了，因为Coding默认支持Jekyll搭建网站）
7. 域名添加解析，这时要把之前的github的解析改成境外，把coding的设置为默认。
## 参考来源
- [hexo史上最全搭建教程 作者：zjufangzh](https://blog.csdn.net/sinat_37781304/article/details/82729029)<br>
- [Coding Pages 搭建 Hexo 静态博客 作者：Zelsonia](https://www.jianshu.com/p/a19962485b4a)