# 简单的 Git 使用


- [简单的 Git 使用](#简单的-git-使用)
  - [初级使用方法](#初级使用方法)
    - [Clone](#clone)
    - [Remote](#remote)
  - [`项目分叉历史`](#项目分叉历史)
  - [分支操作](#分支操作)
  - [连接远程仓库](#连接远程仓库)
  - [设置 ssh 密钥连接](#设置-ssh-密钥连接)


## 初级使用方法
### Clone
```
1.  git clone URL

2.  git fetch / merge

3.  git commit -a -m 

4.  git push -u origin main
```
### Remote
```
1.  git remote add origin URL

2.  git fetch / merge

3.  git checkout -b main

4.  git pull origin main

5.  git commit -a -m 

6.  git push -u origin main
```


## `项目分叉历史`
```
$   git log --oneline --decorate --graph --all
```
## 分支操作
```
创建新的分支
$   git branch <new-branch-name>

切换到新的分支
$   git checkout <new-branch-name]

创建分支并切换到新分支
$   git checkout -b <new-branch-name>

删除分支
$   git branch -d <branch-name>

查看已经合并的分支
$   git branch --merged
```
## 连接远程仓库
```
连接远程仓库
$   git remote add origin URL

获取远程库的分支
$   git fetch

获取远程库的文件
$   git pull

第一次提交并且设置远程仓库为分支上游
$   git push -u origin main
```

## 设置 ssh 密钥连接
```
产生公钥
$   ssh-keygen -t rsa -C "your-email"
```


