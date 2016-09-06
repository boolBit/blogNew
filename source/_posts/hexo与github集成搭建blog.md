---
title: hexo与github集成搭建blog
tags: [hexo, git, blog]
categories: 教程
---

之前在csdn，博客园上写博客，想着一方面总结学过的知识，另一方面可以提高自己的写作总结能力。很多知识点理解运用并不难，但是把逻辑清楚的讲解给别人听，还是需要一定的技巧和经验。[Hexo](https://hexo.io/zh-cn/)是一款开源的Node.js静态博客框架，支持markdown语法，主题很丰富。之前在阿里云上搭过，可惜写了几篇没有坚持下来。痛下心痛决定结合github pages重新开始...

----------


###  配置环境
 - git
   安装好git，生成sshkey，在github中配置好sshkey，可以推送拉取仓库。
 - github 
   需要一个github账号，创建一个username.github.io仓库，用来存放UserPage， 可以通过http(s)://username.github.io 访问。这里存放我们的nodejs项目主页index.html。
 - nodejs
  用于安装HEXO，依赖。
 - markdown编辑器 
  熟悉markdown基本语法，在hexo生成的文章中编写博客。win环境下可以使用Cmd markdown 编辑器编写，支持即时预览功能。
 
###  hexo安装

```
$ npm install -g hexo-cli
$ hexo init <folder>
$ cd <folder>
$ npm install
$ npm install hexo-deployer-git --save
```
- 安装完成后目录如下：
> .
 ├── _config.yml(网站全局配置信息，例如标题，关键词，配置文章、目录基本信息，选择主题，配置deploy信息等)
 ├── package.json （应用程序信息）
 ├── scaffolds（文章模板文件夹）
 ├── source （资源文件夹，markdown和html文件会被生成到public文件夹，其它会被拷贝过去）
 |   └── _posts （为一个页面目录，存放默认发表的文章）
 └── themes （存放主题插件和配置）

### 基本命令
  * new
  `hexo new <layout> pageName `
    新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout           参数代替。如果标题包含空格的话，需用引号括起来。
  * server
 本地启动nodejs服务器，可通过url访问
  * generate
    生成静态页面和css等
  * publish
    生成草稿 （hexo --draft 显示草稿）
  * clean
    清除缓存文件 (db.json) 和已生成的静态文件 (public)。
  * deploy
    部署public目录下的文件到配置的对应目的地（ 参数-g 首先执行generate页面）

### 自定义主题
  选择一个心仪的[主题](https://hexo.io/themes/),进入主题github主页，按照教程进行相应配置即可。也可在此基础上进行自定义开发。

- 部署到github
  编辑_config.yml文件

```
deploy:
    type: git
    repo: https://github.com/githubusername/githubusername.github.io.git （需要一个 用户名.github.io 为名称的github项目，用于deploy generate后的public目录。）
    branch: master
```
执行 `npm install hexo-deployer-git --save` 此外需要在github上配置了本机的ssh key。


```
hexo new "page" 创建一片新文章
hexo clean
hexo deploy -g
```
浏览器访问githubusername.github.io即可看到相应的页面。

### 分布式开发
  Hexo部署到GitHub上的文件，是.md（文章）转化之后的.html（静态网页）。如果需要在多台电脑上写作或者备份博客，可以将整个博客保存到github仓库中。首先将整个blog项目init成一个git项目，在github上创建一个新的blog项目，设置好远程跟踪。如果主题theme是通过git clone方式安装的，需要删除隐藏的.git目录，相应theme文件夹才会被推送到远程，不然推送上去的theme子文件夹是一个空目录。
  登入另外一台电脑后，需要安装好环境，clone博客项目，执行 `npm install`， `npm install hexo-deployer-git --save` 即可。

### 绑定域名
 我们已经有课可以通过username.github.io访问的博客，如果需要使用自定义的域名，可以在[godaddy](https://sg.godaddy.com/zh/offers/default.aspx)中购买一个。此外需要在source文件夹中创建一个CNAME文件，里面填写你的根域名例如baidu.com. 当dns解析到github服务器中，会根据http请求头中你的域名找到对应的Github Pages项目。
进入管理域名面板，域名详细面板。有多种配置方式

1. 使用免费的转址服务，可以隐藏转址后的url。但是手机上访问，会出现无法适配手机屏幕，展示的是电脑页面的情况。
2. 添加两条A记录DNS，主机类型为@（根域名）分别指向192.30.252.153，192.30.252.154. 
3. 添加两条CNAME记录，主机类型分别为www和@指向username.github.io
4. 使用腾讯的DNSpod免费dns服务。选择自定义域名服务器，填写dnspod的域名服务器，在dnspod后台进行配置

### SEO
参见CSDN[博文](http://blog.csdn.net/zaoan_wx/article/details/50859675)