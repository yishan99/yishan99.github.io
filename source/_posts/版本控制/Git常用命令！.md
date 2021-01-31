---
title: Git常用命令！
date: 2021-01-19 10:50:15
tags:
- 版本控制
---
### Git常用命令

<!--more-->

#### 本地仓库

##### 1.操作入门

- 用git init初始化，创建 git 仓库

- git statuss查看 git 状态 （文件是否进行了添加、提交操作）

- git add 文件名添加，将指定文件添加到暂存区

- git commit -m '提交信息'提交，将暂存区文件提交到历史仓库

- git log查看日志（ git 提交的历史日志）

- git reflog ：可以查看所有分支的所有操作记录（包括已经被删除的 commit 记录的操作）

- 需求: 将代码切换到第二次修改的版本

  指令：git reset --hard 版本唯一索引值

##### 2.分支管理操作

- 创建和切换

  创建命令：git branch 分支名
  切换命令：git checkout 分支名

- 新分支添加文件

  查看文件命令：ls

  总结：不同分支之间的关系是平行的关系，不会相互影响

- 合并分支

  合并命令：git merge 分支名

- 删除分支

  删除命令：git branch -d 分支名

- 查看分支列表

  查看命令：git branch

#### 远程仓库

生成SSH公钥步骤

1. 设置Git账户

   - git config user.name（查看git账户）
   - git config user.email（查看git邮箱）
   - git config --global user.name “账户名”（设置全局账户名）
   - git config --global user.email “邮箱”（设置全局邮箱）
   - cd ~/.ssh（查看是否生成过SSH公钥）

   2.生成SSH公钥

   - 生成命令: ssh-keygen –t rsa –C “邮箱” ( 注意：这里需要敲3次回车)
   - 查看命令: cat ~/.ssh/id-rsa.pub

   3.公钥测试

   - 命令: ssh -T git@gitee.com

   

   推送到远程仓库

   - 步骤
     1. 为远程仓库的URL（网址），自定义仓库名称
     2. 推送
   - 命令
     git remote add 远程名称 远程仓库URL
     git push -u 仓库名称 分支名

   