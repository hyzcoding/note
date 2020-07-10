# Git
## 简介

开源的分布式版本控制系统

[github](https://www.github.com)
[gitee](https://www.gitee.com)



## 安装

```bash
> apt-get install git
> git --version
```

## 使用

```bash
## 基本操作
git init myproject						 # 初始化当前目录为项目
git add README.md						 # 添加
git status -s							 # 查看文件是否修改
git diff     							 # 查看不同
git commit -m 'discription'				 # 提交
git commit -am 'discription'			 # 添加并提交
git rm -f <file>						 # 移除文件
git mv README  README.md				 # 移动或重命名

## 分支操作
git branch								 # 列出分支
git branch (branchname) 				 # 创建分支
git checkout (branchname)				 # 切换分支
git branch -d (branchname)				 # 删除分支
git merge								 # 合并分支

git log --oneline						 # 查看历史

## 远程操作
git clone <repo> <directory>			 # 克隆到指定目录

```

