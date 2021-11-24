# Git

[Git](https://git-scm.com/)：版本管理工具，主要是对于纯文本文件（代码）。

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