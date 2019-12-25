---
layout: post
title: 'Learn Git Branching 总结'
cover: https://raw.githubusercontent.com/sxxjaeho/assets/master/assets/images/imagesSnip20191225_1.png
date: 2019-2-14
categories: Git
tags: Git
---


# Learn Git Branching 总结
链接：https://learngitbranching.js.org

## **主要**
####  基础篇
###### 1.Git Commit
```c
git commit
git commit
```
###### 2.Git Branch
```c
git branch bugFix
git checkout bugFix
```
###### 3.Git Merge
```c
git checkout -b bugFix
git commit
git checkout master
git commit
git merge bugFix
```
###### 4.Git Rebase
```c
git checkout -b bugFix
git commit
git checkout master
git commit
git checkout bugFix
git rebase master
```
#### 高级篇
###### 1.分离 Head
```c
git checkout c4
```
2.相对引用 ^
最优解：

```c
git checkout bugFix^
```
其他解：

```c
git chekcout bugFix
git checkout HEAD^
```
###### 3.相对引用2 ~
```c
git branch -f master c6
git branch -f bugFix c0
git checkout c1
```
###### 4.撤销变更
reset: (local)
`git reset`通过把分支记录回退几个提交记录来实现撤销改动。你可以将这想象成“改写历史”。`git reset` 向上移动分支，原来指向的提交记录就跟从来没有提交过一样。

revert: (pushed)
虽然在你的本地分支中使用`git reset`很方便，但是这种“改写历史”的方法对大家一起使用的远程分支是无效的哦！为了撤销更改并分享给别人，我们需要使用`git revert`。

```c
git reset HEAD^
git checkout pushed
git revert HEAD
```

#### 移动提交记录
##### 1.Git Cherry-pick
```c
git cherry-pick c3 c5 c7
```
##### 2.交互式 rebase
```c
git rebase -i HEAD~4
```

#### 移动提交记录
##### 1.只提取一个提交记录
```c
git rebase -i HEAD~3
git branch -f master bugFix
```
或

```c
git cherry-pick bugFix
git branch -f master bugFix
```
##### 2.提交的技巧 #1
```c
git rebase -i HEAD~2 #修改C2和C3的顺序
git commit --amend
git rebase -i HEAD~2 #修改C3'和C2''顺序
git branch -f master
```
##### 3.提交的技巧 #2
```c
git checkout master
git cherry-pick newImage
git commit --amend
git cherry-pick caption
```
##### 4.Git Tag
```c
git tag v0 c1
git tag v1 c2
git checkout c2
```
##### 5.Git Describe
由于标签在代码库中起着“锚点”的作用，Git 还为此专门设计了一个命令用来描述离你最近的锚点（也就是标签），它就是 `git describe`！

Git Describe 能帮你在提交历史中移动了多次以后找到方向；当你用 `git bisect`（一个查找产生 Bug 的提交记录的指令）找到某个提交记录时，或者是当你坐在你那刚刚度假回来的同事的电脑前时， 可能会用到这个命令。

git describe 的语法是：

```c
git describe <ref>
```

\<ref> 可以是任何能被 Git 识别成提交记录的引用，如果你没有指定的话，Git 会以你目前所检出的位置（HEAD）。

它输出的结果是这样的：

```c
<tag>_<numCommits>_g<hash>
```
tag 表示的是离 ref 最近的标签， numCommits 是表示这个 ref 与 tag 相差有多少个提交记录， hash 表示的是你所给定的 ref 所表示的提交记录哈希值的前几位。

当 ref 提交记录上有某个标签时，则只输出标签名称。

```c
git commit
```

#### 高级话题
##### 1.多次 Rebase
```c
git rebase master bugFix
git rebase bugFix side
git rebase side another
git branch -f master another
```
##### 2.两个父节点
```c
git branch bugWork HEAD~^2~
```
##### 3.纠缠不清的分支
```c
git checkout one
git cherry-pick c4 c3 c2
git checkout two
git cherry-pick c5 c4 c3 c2
git branch -f three c2
```


**整理到最后发现可以看答案**
......

```c
show solution
```
**还是非常合理的**
......
**我还要不要继续整理了**
......
**再过一遍吧**
......

---

## **远程**

#### Push & Pull —— Git 远程仓库！
###### 1.Git Clone
```c
git clone
```
###### 2.远程分支
```c
git commit
git checkout o/master
git commit
```
###### 3.Git Fetch
```c
git fetch
```
###### 4.Git Pull
`git pull`就是`git fetch`和`git merge <just-fetched-branch>`的缩写。

```c
git pull
```
###### 5.模拟团队合作
```c
git clone
git fakeTeamwork 2
git commit
git pull
```
###### 6.Git Push
```c
git commit
git commit 
git push
```
###### 7.偏离的提交历史
`git pull --rebase`就是`fetch`和`rebase`的简写。

```c
git clone
git fakeTeamwork 1
git commit
git pull --rebase
git push
```


#### 关于 origin 和它的周边 —— Git 远程仓库高级操作
###### 1.推送主分支
```c
git fetch
git rebase o/master side1
git rebase side1 side2
git rebase side2 side3
git rebase side3 master
git push
```
###### 2.合并远程仓库
```c
git checkout master
git pull origin master
git merge side1
git merge side2
git merge side3
git push origin master
```
###### 3.远程追踪
```c
git checkout -b side o/master
git commit
git pull --rebase
git push
```
###### 3.远程追踪
```c
git branch -f side master
git commit
git pull --rebase
git push
```
###### 4.Git Push的参数
```c
git push origin master
git push origin foo
```
###### 5.Git Push的参数 2
```c
git push origin foo:master
git push origin master^:foo
```
###### 6.Git Fetch的参数
```c
git fetch origin master^:foo
git fetch origin foo:master
git checkout foo
git merge master
```
或

```c
git pull origin master^:foo
git pull origin foo:master
git branch -f foo
git checkout foo
```
###### 7.没有source的source
```c
git pull origin :bar
git push origin :foo
```
###### 8.Git Pull的参数
```c
git pull origin bar:foo
git pull origin master:side
```


