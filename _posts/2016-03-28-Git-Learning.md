---
layout:     post
title:      "Git Learning"
subtitle:   "Git code(Basic)"
date:       2016-03-28
author:     "see"
header-img: "img/in-post/header/post-bg-work.jpg"
tags:
    - Git
    - Study
---

# Gitnotes

## Git Tutorial - 2 - Config Our Username and Email
1. "git config --global user.name "name"" 设置用户名
2. "git config --global user.email "email" " 设置邮箱
3. "git config --list" list all of the settings
4. "clear" clean the screen
5. "git help"  

## Git Tutorial - 3 - Creating Our First Repository
1. "pwd" 显示工作路径  
2. "cd .." 返回上级工作路径  
3. "ls" list当前路径做包含的文件夹
4. "cd Tuna" 设置Tuna文件夹为工作路径  

## Git Tutorial - 4 - Commit
1. "git init" reinitialized existing git repository  
2. "git add ." 将文件添加到staging area  
3. "git commit -m "some detailed message" "  将文件从staging area提交到repository   

## Git Tutorial - 5 - The Commit Log
1. "git log" review commit history  
2. "git log --author="See" "
3. "git status"  

## Git Tutorial - 8 - Viewing the Changes That You Made
1. "git diff" show the difference between working copy and repository  

## Git Tutorial - 9 - Comparing the Staging Area with the Repository
1. "git diff --staged" show the difference between staging area and repository  

## Git Tutorial - 10 - How to Delete Files  
1. "git rm xxx.txt" remove xxx.txt files  

## Git Tutorial - 11 - How to Move and Rename Files  
1. "git mv xxx.txt yyy.txt" renamed xxx.txt to yyy.txt
2. "git mv xxx.txt yyy/zzz.txt" move xxx.txt to yyy folder and named as zzz.txt  

## Git Tutorial - 14 - Checkout 
1. "git checkout -- xxx.txt" 从repository恢复xxx.txt最新版本文件  

## Git Tutorial - 15 - Unstage Files
1. "git reset HEAD xxx.txt" take xxx.txt file from staging area to working copy  

## Git Tutorial - 16 - Getting Old Versions from the Repository
1. 通过git log命令找到要恢复文件的commit序列号
2. "git checkout -- xxxx yyy.txt" xxxx is the number，恢复yyy.txt,序列号为xxxx.

## Git Tutorial - 18 - Pushing to a GitHub Repository
1. "git remote add reponame repo的网址" 建立远程连接
2. "git push -u reponame master" 把working copy的文件推送到github repo，记住在这之前要把本地文件用git提交保存。
