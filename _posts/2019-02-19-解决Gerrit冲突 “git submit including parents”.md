---
layout: post
author: delims
categories: iOS
---

>出现问题原因：commit相互依赖。具体讲就是：gerrit上已经存在commit A（commit A还未merge入库），然后你在commit A的代码基础上进行了修改（划重点，基于A修改！），并做了新的commit B，commit B已经包含了commit A的修改，于是在gerrit 上abondon commit A，只留下commit B在gerrit上，这样一来，commit B review通过后做merge时你就会得到标题中的错误。


#解决方法：

1. 从远程分支上重新创建一个新的工作分支：git fetch origin master(远程分支):new_work(新分支)

2. 切换到新的工作分支：git checkout new_work

3. 将commit B 移到新分支上（gerrit 页面右上角download中直接复制cherry-pich命令）：git fetch ssh://xxx xxx && git cherry-pich xxx

4. 正常解决冲突流程，不做复述

5. 正常提交代码：git push origin HEAD:refs/for/mater(需要提交到的分支)

6. 刷新gerrit，重新做code review。


原文地址:https://blog.csdn.net/qq_35168368/article/details/83109016
