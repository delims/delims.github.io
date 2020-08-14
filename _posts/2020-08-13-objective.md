---
layout: post
author: delims
categories: iOS
---

> 几个方法。

- +(void)load 类装载时候执行。
-  +(void)initialize 类第一次收到消息时候执行。

> NSObject

// hash和地址一样，占用空间8字节。原因应该是只有一个isa指针。

```
NSObject *object = NSObject.new;
NSLog(@"%lx",(unsigned long)object.hash);
NSLog(@"%p",object);
NSLog(@"%lu",sizeof(object));
```
> isa 和 meta class

- 实例的 isa 指向对应的类。
- 类的 isa 指向**元类**。
- 每个类都有一个对应的**元类**，用class_getName()获取到的元类名和类相同，但是在内存中不是同一对象。
- 元类继承对应类的父类的元类。
- 根类的元类的superclass竟然是根类，isa指针指向自己。
- 看来根类是所有类的老爷爷类，所有的类都集成自根类。尽管根类的isa指向了自己的孩子。

```
struct method_t {
    SEL name;
    const char *types;
    MethodListIMP imp;
	
    struct SortBySELAddress :
        public std::binary_function<const method_t&,
                                    const method_t&, bool>
    {
        bool operator() (const method_t& lhs,
                         const method_t& rhs)
        { return lhs.name < rhs.name; }
    };
};

```
