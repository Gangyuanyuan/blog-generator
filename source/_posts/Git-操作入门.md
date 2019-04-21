---
title: Git 操作入门
date: 2019-04-20 13:02:42
tags: git
categories: 前端探索
---

Git Bash 有很多命令，git 只是其中一种。
我们首先要配置好 GitHub（此处省略），再来配置 git。

## 配置 git
打开 Git Bash，依次运行下面五句话来配置 git：
```
git config --global user.name 英文名
git config --global user.email 邮箱
git config --global push.default matching
git config --global core.quotepath false
git config --global core.editor "vim"
```
前两句命令中，把 "英文名" 要改成你自己的名字，"邮箱" 要改成你自己的邮箱。

## git 的三种使用方式
git 有三种使用方式，按需求选择使用哪一种都可以。
+ 只在本地使用
+ 将本地仓库上传到 GitHub
+ 下载 GitHub 上的仓库

以下讲一下 git 在本地的使用方法，以及如何将本地仓库上传到 GitHub。

## git 在本地使用
1. 创建一个目录，以 "git-ex" 为例， `mkdir git-ex`
2. 进入目录 `cd git-ex`
3. `git init` 初始化本地仓库

![git init](https://upload-images.jianshu.io/upload_images/13038962-55f15d94a083bb4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

执行此命令后，会在 git-ex 里创建一个 .git 目录，它就是一个本地仓库，是默认的隐藏文件，可以用 `ls -al` 查看。
![查看生成的文件](https://upload-images.jianshu.io/upload_images/13038962-27cd8500369ff4f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. 在 git-ex 目录里面添加任意文件，假设我们添加了两个文件 mygit.html 和 ex.txt
`touch mygit.html`
`touch ex.txt`
5. 用 `git add` 将文件添加到“暂存区”
```
$  git add 文件路径
```
+ 可以执行两个命令分别添加文件
`git add mygit.html`
`git add ex.txt`
+ 也可以一次添加所有文件
`git add .`
 即把当前目录里的变动都添加到了“暂存区”。
![git add](https://upload-images.jianshu.io/upload_images/13038962-dfd5c4b87959cff5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6. 用 `git commit` 将 add 过的内容正式提交到本地仓库（.git就是本地仓库），并添加一些注释信息，方便以后查看。
```
$  git commit 文件路径 -m '备注信息' 
```
+ 可以分别提交文件：
`git commit mygit.html -m '添加 mygit.html'`
`git commit ex.txt -m "添加 ex.txt"`
+ 也可以一次性提交所有文件：
`git commit . -m "添加了几个文件"`

7. + `git status -sb` 显示当前所有文件的状态，看有没有记录在仓库里。
`-s` 的意思是显示总结（summary）
`-b` 的意思是显示分支（branch）

操作过程中可以随时用这个命令查看文件状态：
> `??`：代表还没有对这些文件进行操作
`A`：Add，代表文件已被添加进仓库
`M`：Modified，代表这个文件被修改了

8. + `git log` 查看变更历史

9. 若文件有新的变动，则需要再次将改动添加到暂存区，并提交到 .git本地仓库，执行 `git add` 和 `git commit` 两个命令就行了。

10. 这样整个过程就完成了。

11. 另外，我们来看一下` git commit -v ` 
（1）`-v`选项：会将所做改变的diff输出放到编辑器中，从而使你知道本次提交做了哪些修改，diff输出的行前缀为＃。
（2）所以使用`git commit -v`来提交内容时，会启动文本编辑器要求输入提交说明，此时只需输入说明，然后保存并退出。退出编辑器时，Git会丢掉注释行，用你输入的提交附带信息生成一次提交。若输入为空，则本次操作不会有结果。
![git commit -v](https://upload-images.jianshu.io/upload_images/13038962-0b76f51e8be2390b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 将本地仓库上传到 GitHub
1. 在 GitHub 上新建一个空仓库，名称随意，一般跟本地目录名一致，还以 "git-ex" 为例。
2. 填完仓库名，不要动其他选项，直接点即 "Create repository" 创建新仓库。
3. 在新页面点击 "SSH" 按钮（**注意**：最好使用 SSH 地址，使用HTTPS 地址后期会很麻烦）。
4. 我们已经有本地仓库了，根据提示分别复制运行这两行代码： 
`git remote add origin https://github.com:/xxx/git-ex.git`
`git push -u origin master`
5. 刷新当前页面，本地仓库就上传到 GitHub 了。

**使用 GitHub Pages 预览 HTML网页**
6. 在此页面点击 Settings，往下滚动页面，在 source 处选中 "master branch" 然后点击 "Save" 保存。
7. source 上方会出现一个预览链接，访问`https://xxxxxxxxxxxxxx/mygit.html` 即可预览 mygit.html 网页。

## 总结
### 在本地使用
`cd xxx`   （注意要先进入这个目录）
`git init `
`git add 文件路径 `
`git commit -m "备注信息"`
### 将本地变动上传到 GitHub
`git add 文件路径`
`git commit -m "备注信息"`
`git pull`    （push 之前记得先 pull）
`git push`
上传更新时， 如果远程被更改了，而本地不知道，在 push 之前就必须先 pull