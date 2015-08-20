title: hexo-github
date: 2015-08-20 18:07:43
tags: blog
categories: hexo
---
# 使用github+hexo创建个人博客

------

> * github的配置
> * hexo的配置
> * hexo部署到github中
> * 在xxx.github.io中编写博客
> * 主题网站设置

------

## hexo的配置

### 安装hexo
打开git bash,输入：
```
$ hexo init
```
### 部署hexo
在电脑中建立一个文件夹存放hexo相关文件如`D:/hexo`,在这个文件夹下右击打开`git bash`输入：
```git
$ hexo init
```
进行`npm`的安装：
```git
$ npm install
```

在本地查看之前需要进行`server`的下载安装：
```
$ npm install hexo-server
```
进行生成：
```
$ hexo g
```
利用`hexo s`开启本地调试。在浏览器中输入`localhost:4000`即可查看本地生成的页面。
更改博客网页主题，在github中选择喜欢的主题将其放在`theme`目录下，并更改hexo根目录下的文件`_config.yml`,并将其中的语句`theme`改为对应的主题名称,如`jacman`：
```
# Extensions
## Plugins: http://hexo.io/plugins/
## Themes: http://hexo.io/themes/
theme: jacman
```
在git bash下关闭原先的调试进程`Ctrl+C`,重新进行调试`hexo s`。
## hexo部署到github中
在hexo3.0中需要安装`hexo-deployer-git`才能部署到github中，安装git：
```
$ npm install hexo-deployer-git --save
```
在hexo项目中更改其根目录下的文件`_config.yml`，添加如下内容：
```
deploy:
    tpye: git
    repo: <repository url>
    branch: [branch]
    message: [message]
```
在GitHub网站中新建一个repository，命名为`your_github_name.io`。
repo: GitHub地址
branch: 分支名称
message: Customize commit message
例如`_config.yml`修改为：
```
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/ShenhuaHou/ShenhuaHou.github.io.git
  branch: master
```
最后进行部署：
```
$ hexo deploy
```
## 在xxx.github.io中编写博客
在`hexo`项目目录`\source\_posts`下手动添加``.md`文件进行编辑，或者在`git bash`下输入：
```
$ hexo n "blog name"
```
## 主题网站设置
这里选择`hexo-theme-next`，针对该主题相关的设置见[here](http://theme-next.iissnan.com/ "theme-next")。

