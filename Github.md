# Git

[Git](https://git-scm.com/)：版本管理工具，主要是对于纯文本文件（代码）。

## 安装与配置

安装：
- 添加到环境变量，命令`git`。

配置邮箱和用户名：
```bash
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```
查看：
```bash
git config --global user.email
git config --global user.name
```

## 初始化

```bash
cd your_dir
git init
```
- 初始化之后生成`.git/`隐藏目录。
- 已经是仓库就不需要再初始化。

## 暂存

```bash
git add your_file
```
- 添加到git的暂存区。
- 添加所有文件：`git add *`。

## 查看状态

```bash
git status
```

## 提交

提交暂存的内容：
```bash
git commit -m "your commit message"
```

## 推送到远程仓库

Github中的仓库是远程仓库，其实和本地仓库并没有区别，只是能够在网页访问和通过各种网络协议(https/ssh/git)访问。

```bash
git push github master
```
- `github`是要推送到的远程仓库名称。
- `master`是要推送的分支（master是主分支）。

## 远程仓库

- Github上新建仓库。
- 得到仓库地址`git@github.com:T-a-pirate/CSNotes.git`，常用ssh协议。
- 将仓库地址添加到本地版本库的远程地址：
```bash
git remote add github git@github.com:T-a-pirate/CSNotes.git
```
- 展示版本库的远程仓库：
```bash
git remote show github
```
- 提交本地修改到远程仓库：
```
git push github [master]
```

## 配置SSH Key

Github, Setting, SSH And GPG Keys：将本地生成的SSH Key添加到其中。

生成SSH Key：
```bash
ssh-keygen -t rsa -C  youremail@example.com
```
- 生成了文件：
```
C:\Users\Hua_Gui\.ssh\id_rsa
C:\Users\Hua_Gui\.ssh\id_rsa.pub
```
- `id_rsa`私钥，`id_rsa.pub`公钥。
- 将`id_rsa.pub`内容复制到Github的SSH Key中即可。

## VSCode的Git集成

暂存：
- 版本管理栏，更改文件中点击 + 号暂存。

提交：
- 输入提交信息，提交到版本库，点击 √ 提交。

推动：
- `...`/Pull Push/Push to...，点击版本库就推送上去了。
- `Ctrl+Shift+P`，`git push to`，点击版本库。