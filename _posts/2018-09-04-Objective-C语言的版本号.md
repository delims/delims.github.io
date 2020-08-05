---
layout: post
author: delims
categories: iOS
---

# 前言
 每次阅读 runtime 的头文件时看到`__OBJC2__ `时就很疑惑，这到底是个什么？从字面意思来看像是 Objetive-C 语言的 2.0 版本。我们都听说过 swift 语言的版本号，但是却很少听说 Objetive-C 的版本号。
那么问题来了
 
**Objetive-C 语言一共有几种版本？分别运行在什么系统上？**

百度`__OBJC2__`找不到任何这方面的答案，每当此时我就很苦恼，我觉得我应该找到这个答案。


终于在 [stackoverflow](https://stackoverflow.com/questions/7115325/how-do-i-know-what-version-of-objc-language-am-i-using/7115351#7115351) 中找到了我所需要的答案，于是豁然开朗

# 帖子原文
**Q：How do i know what version of ObjC language am i using?**

This is kind of a silly question, but really, how can i tell whether i am using ObjC version 2 or something else?

I can always assume "the latest", but i'd rather know :)

Is there a command line check i can run? Please advise

**A:If you are running 10.5 or later, or any version of iOS, your computer is running Objective-C 2. If you are writing code which you want to work on systems before this, you can check for the __OBJC2__ macro, which will be defined only for Objective-C 2 and later systems.**

```
#ifdef __OBJC2__
// use objective-c 2
#elif defined(__OBJC__)
// use objective-c 1
#else
// no objective-c
#endif
```

原文地址 [https://stackoverflow.com/questions/7115325/how-do-i-know-what-version-of-objc-language-am-i-using/7115351#7115351)](https://stackoverflow.com/questions/7115325/how-do-i-know-what-version-of-objc-language-am-i-using/7115351#7115351))

# 总结

- Objective-C有两个版本 1.0 和 2.0。
- `__OBJC2__` 为真代表当前使用的是 Objective-C 2.0
- macOS 10.5 及以后的版本使用的是 Objective-C 2.0 ，版本号小于 10.5 的系统是Objective-C 1.0
- 任何iOS系统都是 Objective-C 2.0
- 可以推断出在 iOS 系统 中`__OBJC2__` 永远为YES



在此十分感谢 stackoverflow，感谢 [ughoavgfhw](https://stackoverflow.com/users/458390/ughoavgfhw)，虽然这不是一个什么高深的问题，但是它解开了一个长期存在我心中的疑惑。
