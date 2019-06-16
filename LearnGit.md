[TOC]

# Learn git

## 初始化
```bash
git init #初始化库

//添加文件
git add <file>
git commit -m "add a file"
//修改文件
git diff   //查看工作区和版本库最新内容的差别
```

## 查看库和日志
```bash
git status //查看仓库当前状态

git log //查看提交日志
git log --pretty=online
git reflog //查看最近的命令
```

## 回退、撤销
```bash
git reset --hard HEAD^ //回退到上一版本
git reset --hard <commit id> //回退到指定版本

git checkout -- readme.txt// 1.修改后未放入暂存区，则回到和版本库一样 2.放入暂存区后又作修改，则回到添加到暂存区后的状态
git reset HEAD readme.txt //将暂存区的修改撤销，回到工作区


```



## 删除文件

```bash
// 1. 确认删除
git rm test.txt
git commit -m "remove test.txt"

// 2. 误删文件
git checkout -- test.txt

```



## 远程仓库

###  将本地的homeRepo和GitHub上的StephenEvenson/homeRepo关联

```bash
git remote add origin git@github.com:StephenEvenson/homeRepo

```



### 第一次推送本地master

```bash
git push -u origin master

```

如果报错


>! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:StephenEvenson/homeRepo'



表示远程仓库有东西，先git pull

```bash
git pull --rebase origin master

```

然后再推送



### 第二次开始推送

```bash
git push origin master

```

### 将远程仓库克隆到本地

```bash
git clone git@github.com:StephenEvenson/gitskills.git

```



## 分支

### 创建分支

```bash
git checkout -b dev # -b表示创建并切换
//相当于以下两条命令
git branch dev
git checkout dev

//查看分支
git branch

```



### 合并分支

```bash
git merge dev

//删除分支
git branch -d dev

git branch -D feature #使用-D强行删除未被合并的分支

```



### 解决冲突

当在master和其他分支下对同一文件有不同的修改时，就会出现分支合并冲突，git会在文件中标明冲突的内容

> <<<<<<< HEAD
>
> Creating a new branch is quick & simple.
>
> ||||||| merged common ancestors
>
> Creating a new branch is quick.
>
> \==\=====
>
> Creating a new branch is quick AND simple.
>
> \>>>>>>> featurel

解决冲突：

将冲突文件修改后提交

通过`git log --graph --pretty=oneline --abbrev-commit`查看分支合并情况



### 禁用Fast forward合并分支

Fast forword在删除分支后会丢掉分支信息（无法在log中查看commit id），而禁用Fast forward则能在merge时生成新的commit，可以在分支历史上查看分支信息

```bash
git merge --no-ff -m "merge with no-ff" dev

```



### Bug 分支

```bash
git stash #先储藏工作现场

git checkout master #转到出Bug的分支
git checkout -b issue-101 #新建bug分支
//然后修改bug
//合并bug分支，并删除
//转到之前的工作分支
git stash list #查看之前存储的工作内容
git stash apply #恢复之前工作内容，但不删除stash内容
git stash drop #删除存储的工作内容
git stash pop #恢复工作内容并删除存储


```



### 远程库

```bash
git remote #显示远程库，默认名origin
git remote -v #显示远程库的fetch和push地址
git push origin dev #将本地指定分支推送到origin
git checkout -b dev origin/dev #创建远程库的dev分支
git branch --set-upstream-to=origin/dev dev #将本地dev分支与远程origin/dev分支链接
git pull 

```



### 多人协作流程

1. 试图用`git push origin dev`推送自己的修改

2. 推送失败，则用git pull试图合并

3. 合并冲突，则解决冲突，并在本地提交

4. 再用`git push origin dev`推送

   如果提示`no tracking information`则创建本地分支和远程分支的链接



### rebase

将本地未push的分支提交历史整理成直线，查看历史提交变化时更容易

```bash
git rebase

```



## 标签

### 创建标签

```bash
git tag v1.0. #在对应分支打标签

git tag v0.9 8990be2 #给历史对用的commit id打标签

git tag -a v1.0 -m "version 1.0 released" 03f64d9 #用-a指定标签名，-m指定说明文字

```



### 操作标签

```bash
git tag -d v1.0 #删除标签
git push origin v1.0 #推送标签到远程库
git push origin --tag #推送所用未推送的标签到远程

```



删除远程标签

```bash
git tag -d v0.9 #先在本地删除标签
git push origin :refs/tag/v0.9  #再在远程库删除标签

```





## 添加多个远程库

```bash 
git remote rm origin  #删除origin远程库
git remote add github git@github.com:StephenEvenson/homeRepo.git  #关联github上的远程库
git remote add gitee git@gitee.com:StephenEvenson/homeRepo.git  #关联码云上的远程库


```

