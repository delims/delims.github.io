---
layout: post
author: delims
categories: iOS
---

>很久之前整理的，放在这里方便查找。

这些都是objc2的，旧版本的忽略。

```

typedef struct objc_selector *SEL;

typedef struct objc_object   *id;
typedef struct objc_class    *Class;

typedef struct method_t *Method;
typedef struct ivar_t *Ivar;
typedef struct category_t *Category;
typedef struct property_t *objc_property_t;

typedef void (*IMP)(void /* id, SEL, ... */ ); 


```

