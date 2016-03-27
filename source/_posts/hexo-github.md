title: hexo学习-hexo+github搭建静态博客
date: 2015-08-20 23:02:11
tags: [github,hexo]
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
## github的配置
### git创建本地库
windows下下载git并安装
安装完成后设置全局账号：
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
建立一个新文件夹来创建新版本库：
```
$ mkdir learn
$ cd learn
$ git init
```
添加文件：
```
$ git add readme.md
$ git add readme2.md readme3.md
$ git add .
```
编写提交说明：
```
$ git commit -m "add a file"
```
### 远程仓库github与本地仓库同步
先创建SSH key：
```
$ ssh-keygen -t ssa -C "github email"
```
一路回车，使用默认设置，完成之后可以在操作目录下找到`id_rsa`和`id_rsa.pub`文件，将`id_rsa.pub`文件中的公钥添加到github网站设置中：`Settings->SSH Keys->Add SSH key->name & keys`。
在github网页中新建一个远程仓库如`learn`，然后在本地库`learn`目录下：
```
$ git remote add origin https://github.com/yourname/learn.git
```
其中`origin`即为远程库的引用名，之后添加本地库中的文件到远程库中：
```
$ git push -u origin master
```
由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。
此后每次提交只需要使用如下命令即可：
```
$ git push origin master
```
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
### 更改博客网页主题
在github中选择喜欢的主题将其放在`theme`目录下，并更改hexo根目录下的文件`_config.yml`,并将其中的语句`theme`改为对应的主题名称,如`jacman`：
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


