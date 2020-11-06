---
layout: post
author: delims
categories: javascript
---

> 注意点

1. switch语句使用恒等运算符 === 进行比较
2. 10 == "10" 返回 true，值相等但是类型不相等。
3. 在对象方法中， this 指向调用它所在方法的对象。
4. 单独使用 this，则它指向全局(Global)对象。在浏览器中，window 就是该全局对象为 [object Window]:
5. 在函数中，函数的所属者默认绑定到 this 上。在浏览器中，window 就是该全局对象为 [object Window]:
6. 在 HTML 事件句柄中，this 指向了接收事件的 HTML 元素：

```
<button onclick="this.style.display='none'">
点我后我就消失了
</button>
```
- 对象方法中的this是该对象
```
var person = {
  firstName  : "John",
  lastName   : "Doe",
  id         : 5566,
  myFunction : function() {
    return this;
  }
};
```

- ES6 可以使用 let 关键字来实现块级作用域。let 声明的变量只在 let 命令所在的代码块 {} 内有效，在 {} 之外不能访问。

- 使用 var 关键字重新声明变量可能会带来问题。在块中重新声明变量也会重新声明块外的变量：let 关键字就可以解决这个问题，因为它只在 let 命令所在的代码块 {} 内有效。
- JSON 英文全称 JavaScript Object Notation
- var obj = JSON.parse(jsonString);  将json字符串转成json对象。
- var jsonString = JSON.stringify(obj); 将json对象转成json字符串。
- javascript:void(0) 中最关键的是 void 关键字， void 是 JavaScript 中非常重要的关键字，该操作符指定要计算一个表达式但是不返回值

```
<p>点击以下链接查看结果：</p>
<a href="javascript:void(alert('Warning!!!'))">点我!</a>
```

- void()仅仅是代表不返回任何值，但是括号内的表达式还是要运行
- setTimeout(print, 3000);用来设置3秒后执行print函数。
- XMLHttpRequest 常常用于请求来自远程服务器上的 XML 或 JSON 数据


```
var xhr = new XMLHttpRequest();
 
xhr.onload = function () {
    // 输出接收到的文字数据
    document.getElementById("demo").innerHTML=xhr.responseText;
}
 
xhr.onerror = function () {
    document.getElementById("demo").innerHTML="请求出错";
}
 
// 发送异步 GET 请求
xhr.open("GET", "https://www.runoob.com/try/ajax/ajax_info.txt", true);
xhr.send();
```

如果你使用完整的 jQuery 库，也可以更加优雅的使用异步 AJAX：

```
$.get("https://www.runoob.com/try/ajax/demo_test.php",function(data,status){
    alert("数据: " + data + "\n状态: " + status);
});
```

ES6 新增了箭头函数。
当只有一个参数时，圆括号是可选的：

```
(单一参数) => {函数声明}
单一参数 => {函数声明}
```

没有参数的函数应该写成一对圆括号:

```
() => {函数声明}

// ES5
var x = function(x, y) {
     return x * y;
}
 
// ES6
const x = (x, y) => x * y;

```

如果函数部分只是一个语句，则可以省略 return 关键字和大括号 {}，这样做是一个比较好的习惯:

const x = (x, y) =>  x * y ;

JavaScript 函数有个内置的对象 arguments 对象。
argument 对象包含了函数调用的参数数组。


document.getElementById("intro");  通过ID查找html元素
var y=x.getElementsByTagName("p"); 通过标签查找html元素
var x=document.getElementsByClassName("intro"); 通过类名查找

所有的 JavaScript 对象都会从一个 prototype（原型对象）中继承属性和方法：

Date 对象从 Date.prototype 继承。
Array 对象从 Array.prototype 继承。
Person 对象从 Person.prototype 继承。
所有 JavaScript 中的对象都是位于原型链顶端的 Object 的实例。
使用 prototype 属性就可以给对象的构造函数添加新的属性和方法：

```
Person.prototype.nationality = "English";
Person.prototype.name = function() {
  return this.firstName + " " + this.lastName;
};
```
 JavaScript 数字均为 64 位的浮点型。52位有效位，11位指数，1位符号

setInterval() - 间隔指定的毫秒数不停地执行指定的代码。
setTimeout() - 在指定的毫秒数后执行指定代码。

window.clearInterval(intervalVariable) 停止 setInterval() 方法执行的函数代码。参数是上个函数返回值


# HTML 标签学习

- \<wbr\> 设置文本过长时换行的位置
- \<video\> 定义视频，src 指定url
- 短语元素，em 强调内容，strong 更强的强调内容，dfn 定义项目，code 定义代码，samp 样本文本，var 定义变量，cite 定义引用。
- ul 无序列表
- ol 有序列表 
- dir 目录列表
- li 列表项目
- u 文本加下划线
- tt 类似打字机或等宽文本效果
- table 表格
- th 表格表头单元格
- tr 表格中的行
- td 表格中的标准单元格
- thead 表格表头
- tfoot 表格页脚
- tbody 标签表格主体
- textare 多行的文本控件
- sup 上标文本
- sub 下标文本
- summary 有关文档的详细信息
- style 标签用于为 HTML 文档定义样式信息
- strike 加删除线文本
- span 来组合行内元素，以便通过样式来格式化它们。
- source 标签为媒介元素
- small 呈现小号字体效果
- select 元素可创建单选或多选菜单。元素中的 option 标签用于定义列表中的可用选项
- section 文档中的区段
- s 加删除线文本
- ruby 标签定义 ruby 注释
- q 标签定义短的引用。
- p 段落
- i 斜体文本
- progress 标签标示任务的进度
- pre 元素可定义预格式化的文本
- object 添加一个对象
- param 为对象添加参数
- output 不同类型的输出，比如脚本的输出。
- optgroup 标签把相关的选项组合在一起
- noscript 脚本未被执行时的替代文本
- noframes  为那些不支持框架的浏览器显示文本
- nav 导航链接的部分
- meta 提供有关页面的元信息
- menuitem 用户可以从弹出菜单调用的命令
- menu 命令的列表或菜单
- mark  带有记号的文本
- main 标签规定文档的主要内容
- link 链接一个外部样式表
- label 标签为 input 元素定义标注
- keygen 用于表单的密钥对生成器字段
- ins 带有已删除部分和新插入部分的文本
- input 用于搜集用户信息
- iframe 创建包含另外一个文档的内联框架
- hr  创建一条水平线
- header 文档的页眉
- footer 文档页脚
- head 文档的头部，它是所有头部元素的容器
- frameset 定义一个框架集
- frame 定义 frameset 中的一个特定的窗口
- form 标签用于为用户输入创建 HTML 表单
- font 规定文本字体、大小和颜色
- figure 标签规定独立的流内容
- figcaption  标签定义 figure 元素的标题
- fieldset 将表单内的相关元素分组
- embed 标签定义嵌入的内容，比如插件
- dt 标签定义了定义列表中的项目 
- dd 列表中定义条目的定义部分
- dl 定义列表
- div 定义文档中的分区或节
- dialog 标签定义对话框或窗口
- details 描述文档或文档某个部分的细节
- del 文档中已被删除的文本
- datalist 与 input 元素配合使用该元素，来定义 input 可能的值
- colgroup 用于对表格中的列进行组合
- col 为表格中一个或多个列定义属性值
- center 对其所包括的文本进行水平居中
- caption 元素定义表格标题 
- canvas  标签定义图形，比如图表和其他图像



<tt>
这段文本包含 <sup>上标</sup>
</tt>

<p><span>some text.</span>some other text.</p>




