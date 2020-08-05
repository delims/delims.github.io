---
layout: post
author: delims
categories: iOS
---

>平时在阅读一些优秀第三方库代码的时候是不是会遇到__attribute__ ((always\_inline))这样的关键字而懵逼。看了下面的文章相信你会变得更自信。

## aligned 
用于内存字节对齐。

```
struct stu{
    char sex;
    int length; ;
}__attribute__ ((aligned (1)));
//用于内存对齐,只能传入2的N次方，传1，2，4以4字节对齐。传8是8字节对齐。

```
## always\_inline 
声明为内联函数，声明后函数在调用的时候直接复制代码。

```
__attribute__((always_inline)) void inlineFunction(){
	
}
```

## objc\_subclassing\_restricted 
声明类不允许被继承，如果被继承会编译报错

```
__attribute__((objc_subclassing_restricted))
@interface FinalObject : NSObject
@end

```
## objc\_requires\_super
 声明方法重载时需要调用父类的方法,Foundation框架已经定义了这个宏。NS_REQUIRES_SUPER

```
- (void)func __attribute__((objc_requires_super));
//Foundation框架定义了这宏
 #ifndef NS_REQUIRES_SUPER
#if __has_attribute(objc_requires_super)
#define NS_REQUIRES_SUPER __attribute__((objc_requires_super))
#else
#define NS_REQUIRES_SUPER
#endif
#endif
```

## objc_boxable 
这个属性是可以将struct 或者unions 转换成NSValue

```
struct __attribute__((objc_boxable)) _some_struct {
    int i;
};
union __attribute__((objc_boxable)) some_union {
    int i;
    float f;
};
typedef struct __attribute__((objc_boxable)) _some_struct some_struct;


int main() {
    some_struct ss;
    NSValue *boxed = @(ss); //如果去掉objc_boxable这里就会报错
}

```
## constructor / destructor 
在执行文件load和unload时候被调用。

```
__attribute__((constructor))
static void beforeMain(void)
{
    NSLog(@"beforeMain");
}

__attribute__((destructor))
static void afterMain()
{
    NSLog(@"afterMain");
}

```
## enable_if 
这个属性只能用在 C 函数上，可以用来实现参数的静态检查：
```
static void printValidAge(int age)
__attribute__((enable_if(age > 0 && age < 120, "你丫火星人？"))) {
    printf("%d", age);
}

void foo(char c) {
    printValidAge(99);
    printValidAge(199);
}
int main(int argc, const char * argv[]) {
    return 0;
}
```
## cleanup

声明到一个变量上，当这个变量作用域结束时，调用指定的一个函数，如下当string的作用域结束会调用func函数。

```
 __strong NSString *string __attribute__((cleanup(func))) = @"字符串";
 
```
## overloadable
让C语言支持参数重载

```
__attribute__((overloadable)) void logAnything(id obj) {
    NSLog(@"%@", obj);
}

__attribute__((overloadable)) void logAnything(int number) {
    NSLog(@"%@", @(number));
}

__attribute__((overloadable)) void logAnything(CGRect rect) {
    NSLog(@"%@",@(rect));
}
```

## objc\_runtime\_name
用于 @interface 或 @protocol，将类或协议的名字在编译时指定成另一个，如下在编译时候类名会被指定为MyLocalName

```
__attribute__((objc_runtime_name("MyLocalName")))
@interface Message:NSObject

@end

```

## _Noreturn

_Noreturn 关键词出现于函数声明中，指定函数不会由于执行到 return 语句或抵达函数体结尾而返回,（可通过执行 longjmp 返回）
调用函数没有返回值，所以程序就停在函数里面了

```
_Noreturn void stop_now(int i) // 或 _Noreturn void stop_now(int i)
{
    if (i > 0) exit(i);
}

int main(void)
{
    puts("Preparing to stop...");
    stop_now(2);
    puts("This code is never executed.");
}
```
## availability
该availability属性可以放在声明上，以描述相对于操作系统版本的声明的生命周期。考虑假设函数的函数声明f

可用性属性表示f在macOS 10.4中引入，在macOS 10.6中已弃用，在macOS 10.7中已废弃。


```
void f(void) __attribute__((availability(macos,introduced=10.4,deprecated=10.6,obsoleted=10.7)));

```
## deprecated (gnu::deprecated)
废弃的api。

```
void f(void) __attribute__((deprecated("message", "replacement")));

```

