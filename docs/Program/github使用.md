# github的使用


<!--ts-->
   * [github的使用](#github的使用)
      * [参考文献](#参考文献)
      * [现阶段使用github的流程](#现阶段使用github的流程)
         * [配置](#配置)
            * [配置邮箱和用户名](#配置邮箱和用户名)
            * [建立和添加SSHkey](#建立和添加sshkey)
         * [建立repository的流程](#建立repository的流程)
            * [若本地已存在代码](#若本地已存在代码)
            * [本地无代码](#本地无代码)

<!-- Added by: xiejiahao, at:  -->

<!--te-->


## 参考文献
我获取github使用方法的网页有：
>[Git远程操作详解 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)
>详解了 `clone`,`remote`,`fetch`,`pull`,`push`五个`git`远程操作。
>
>*“Git有很多优势，其中之一就是远程操作非常简便。本文详细介绍5个Git命令，它们的概念和用法，理解了这些内容，你就会完全掌握Git远程操作。”*
>
><https://git-scm.com/book/zh/v2>
>
>Book *Git Pro* 的中文版，详细讲解了关于git的一切。
>
>*"The entire Pro Git book, written by Scott Chacon and Ben Straub and published by Apress, is available here."*
>
><http://www.runoob.com/manual/git-guide/>很帅的git实用简介！
>
><http://www.runoob.com/git/git-tutorial.html>菜鸟教程网站的git教程,简明实用！
>
><http://www.runoob.com/w3cnote/git-guide.html>


## 现阶段使用github的流程
如果想在一个从没使用过git的unix环境中建立或使用repo，现阶段使用以下流程：
### 配置
#### 配置邮箱和用户名
配置个人的用户名称和电子邮件地址：
```bash
$ git config --global user.name "runoob"
$ git config --global user.email test@runoob.com
```

#### 建立和添加SSHkey
首先在本地创建ssh key:
```bash
$ ssh-keygen -t rsa -C "your_email@youremail.com"
```
后面的your_email@youremail.com改为你在github上注册的邮箱，之后会要求确认路径和输入密码，我们这使用默认的一路回车就行。成功的话会在~/下生成.ssh文件夹，进去，打开id_rsa.pub，复制里面的key。

回到github上，进入 Account Settings（账户配置），左边选择SSH Keys，Add SSH Key,title随便填，粘贴在你电脑上生成的key。
![image](http://www.runoob.com/wp-content/uploads/2014/05/github-account.jpg)

### 建立repository的流程
#### 若本地已存在代码
1. 在github上建立repository，但不添加README.md文件。
2. ***配置`.gitignore`文件，从存放ignore文件的仓库中复制一个合适的并修改。
3. 在本地文件夹中:
    1. `git init`
    2. `git remote add origin 地址`
    3. `git add .`添加所有文件。*实际上最好添加需要ignore的文件*
    4. `git commit -m '信息'`
    5. `git push -u origin master`第一次push时加上`-u`可以将本地master与远程进行链接，之后的push才可以直接使用`git push`.
4. 在远程的仓库中添加`README.md`

#### 本地无代码
在远程建立后直接克隆即可。


### 在已建立的repository中添加.gitignore文件并启用



