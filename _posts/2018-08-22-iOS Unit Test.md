---
layout: post
author: delims
categories: iOS
---

# 前言

用Xcode创建一个iOS项目时会提示是否勾选 **Unit Tests** 和 **UI Tests**，勾选后创建项目自动生成如下两个target:

**(项目名)+Tests** 

**(项目名)+UITests** 

我填写的项目名是`Test_3rd_party_Library`, 展开目录Test\_3rd\_party\_Library可以看到只有一个Test\_3rd\_party\_LibraryTests.m文件和一个info.plist文件。

我们看到.m是一个类，它没有.h文件，它继承自 `XCTestCase `，继承关系：

`Test_3rd_party_LibraryTests` -> `XCTestCase` -> `XCTest` -> `NSObject`

`XCTest` 还有一个子类 `XCTestSuite`,`XCTestSuite` 是测试用例的集合

`XCTestCase` 是定义测试用例、测试方法和性能测试的主要类。

## `XCTestCase`的方法和属性

英文注释是自动创建的，中文的是我加的，我们来看一下

```
#import <XCTest/XCTest.h>

@interface Test_3rd_party_LibraryTests : XCTestCase

@end

@implementation Test_3rd_party_LibraryTests

- (void)setUp {
    [super setUp];
    // Put setup code here. This method is called before the invocation of each test method in the class.  
    // 说白了，就是每次执行一个test开头的方法前都会先用调这个方法
}

- (void)tearDown {
    // Put teardown code here. This method is called after the invocation of each test method in the class.
    //	说白了，就是每次执行完一个test开头的方法后都会调用这个方法
    [super tearDown];
}

- (void)testExample {
    // This is an example of a functional test case.
    // Use XCTAssert and related functions to verify your tests produce the correct results.
    /*
     测试用例方法，这个方法需要满足下面几个条件
     1.开头必须是test
     2.没有参数
     3.没有返回值
     
	  每个test方法前面都有一个菱形，未运行之前是空心的，当我们点击菱形运行之后，如果成功就变成一个对勾，如果失败就变成一个叉号。
	  如果你写了test方法并没有自动生成菱形，那就build一下
    */
}

- (void)testPerformanceExample {
    // This is an example of a performance test case.
    [self measureBlock:^{
        // Put the code you want to measure the time of here.
    }];
    
    /*
    这个方法用来测试代码的性能，将需要测试的代码放到block里面，运行方法，执行结束后会打印运行用时。
     */
}

@end
```

以上几个方法是系统自动添加到子类文件中的。还有几个方法也可以了解一下

```
// 名字随意，开头必须是test
- (void)testPerformanceExample2 {
	
	// 如果需要更细化的定制代码的性能测试，可以用这个方法
	// measureMetrics 是一个字符串数组，包含了可由XCTest度量的性能指标。
	// automaticallyStartMeasuring 指定是否自动执行 startMeasuring 方法。如果为NO，则需要手动在你需要进行性能测试开始的位置加入 [self startMeasuring]，如果为YES，则系统自动执行，我猜测应该是block开始的时候执行，此时如果在手动加入[self startMeasuring]就会报错。
		
    [self measureMetrics:[self class].defaultPerformanceMetrics automaticallyStartMeasuring:NO forBlock:^{

        // Do setup work that needs to be done for every iteration but you don't want to measure before the call to -startMeasuring
        SetupSomething();
        [self startMeasuring];

        // Do that thing you want to measure.
        MyFunction();
        //性能测试结束的位置插入，此方法最多只能执行一次，如果不添加，block结束后自动执行
        [self stopMeasuring];

        // Do teardown work that needs to be done for every iteration but you don't want to measure after the call to -stopMeasuring
        TeardownSomething();
    }];
}

// 在需要测试代码性能开始的地方执行这个方法
- (void)startMeasuring
{
    [super startMeasuring];
}
// 测试结束的时候执行这个方法，
- (void)stopMeasuring
{
    [super stopMeasuring];
}

```

我测试了一下不管运行哪个measureMetrics方法，startMeasuring和stopMeasuring都会成对调用10次，不知道这个10次是怎么来的，如果你知道的话欢迎发邮件给我。
