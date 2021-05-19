---
layout: post
author: delims
categories: iOS
---

> 不知不觉已经是2021年，总感觉现在还是2020年，时间过得太快了！
>  dumpdecrypted 源码读了好几次了。每次读完，时间久了就忘了，也可能是理解的不够深刻。今天重新读了一遍源码，记录一下。


- 首先通过 \_\_attribute\_\_((constructor)) 修饰函数让函数在main函数之前执行，
- 通过函数的第5个参数 ProgramVars 获取到mach_header
- 获取到mach_header 读取 magic参数，判断CPU位数是 64还是32
- mach_header之后就是 load\_commad ，通过 `		lc = (struct load_command *)((unsigned char *)pvars->mh + sizeof(struct mach\_header\_64)); 
` 获取到第一个 load\_command
- 通过 mach_header 获取到 cmd 数量 `pvars->mh->ncmds
- 根据 cmd 数量开启一个for循环，`for (i=0; i<pvars->mh->ncmds; i++)`
- 判断当前 cmd 是不是 LC\_ENCRYPTION\_INFO\_64 或者  LC\_ENCRYPTION\_INFO
- 如果是 将当前cmd 转成 encryption\_info\_command
- 判断加载命令  cryptid 是否为 1，为1代表是加密的
- 通过  encryption\_info\_command 获取 cryptoff 和 cryptsize
- 打开源文件，读取未加密部分写入新文件，
- 写入解密的数据 `			r = write(outfd, (unsigned char *)pvars->mh + eic->cryptoff, eic->cryptsize);
`
- 写入剩余部分数据。
- 把 cryptid 设置为 0
- 关闭输入和输出文件。
- 
