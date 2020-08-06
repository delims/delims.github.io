---
layout: post
author: delims
categories: iOS
---

>很久之前整理的，放在这里方便查找。


```
typedef struct objc_selector *SEL;
typedef struct objc_object   *id;
typedef struct objc_class    *Class;
typedef struct objc_method   *Method;
typedef struct objc_ivar     *Ivar;
typedef struct objc_category *Category;
typedef struct objc_property *objc_property_t;

typedef void (*IMP)(void /* id, SEL, ... */ ); 


```

