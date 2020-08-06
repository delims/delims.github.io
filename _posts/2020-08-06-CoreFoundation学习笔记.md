---
layout: post
author: delims
categories: iOS
---

>做iOS开发一般都使用Foundation框架，很少使用CoreFoundation框架。有一些高级的第三方库中用到一些CoreFoundation框架API的使用。在此总结一些CoreFoundation的使用方法。

- CFStringRef

```
typedef const struct CF_BRIDGED_TYPE(NSString) __CFString * CFStringRef;
```

CFStringRef 是 __CFString 的结构体指针。

使用c字符串创建__CFString

```
const char *c_str = "hello delims";
CFStringRef cfStr = CFStringCreateWithCString(CFAllocatorGetDefault(), c_str, kCFStringEncodingUTF8);
```

CFAllocatorGetDefault()用于获取当前线程的默认分配器

创建字符串后在转成c字符串

```
char buffer[13];
bool succes = CFStringGetCString(cfStr, buffer, 13, kCFStringEncodingUTF8);
if (succes) {
    printf("%s\n",buffer);
}
```
创建一个用于接收c字符串的buffer，buffer必须足够大到可以容纳字符串和字符串结束符。有12个字符，所以至少需要创建长度13的buffer。转成功后返回true。

- CFNumberRef

```
typedef const struct CF_BRIDGED_TYPE(NSNumber) __CFNumber * CFNumberRef;
```

创建 CFNumber

```
int intValue = 1000;
CFNumberRef number = CFNumberCreate(CFAllocatorGetDefault(), kCFNumberIntType, &intValue);
```

- CFMutableDictionaryRef, CFDictionaryRef

这两者都是 __CFDictionary 的结构体指针。

```
typedef const struct CF_BRIDGED_TYPE(NSDictionary) __CFDictionary * CFDictionaryRef;
typedef struct CF_BRIDGED_MUTABLE_TYPE(NSMutableDictionary) __CFDictionary * CFMutableDictionaryRef;
```

和Foundation一样，分可变不可变，创建一个可变版本

```
CFMutableDictionaryRef mutableDictRef = CFDictionaryCreateMutable(CFAllocatorGetDefault(), 0, &kCFTypeDictionaryKeyCallBacks, &kCFTypeDictionaryValueCallBacks);
```

传入0表示存储的元素数量无限。

把创建的number通过cfStr这个key存入mutableDictRef

```
CFDictionarySetValue(mutableDictRef, cfStr, number);
```

从mutableDictRef取出来number，再把number转成int

```
CFNumberRef number2 = CFDictionaryGetValue(mutableDictRef, cfStr);
int fromCFNumber;
CFNumberGetValue(number2, kCFNumberIntType, &fromCFNumber);
printf("%i",fromCFNumber);

```
- CFArrayRef

不解释了，一看就懂。


```
const void *array[] = {cfStr, mutableDictRef};
CFArrayRef arrayRef = CFArrayCreate(CFAllocatorGetDefault(), array, 2, &kCFTypeArrayCallBacks);
CFStringRef stringRef = CFArrayGetValueAtIndex(arrayRef, 0);

```


