---
title: C++ 笔记
date: 2017-01-06 01:12:56
updated: 2017-01-06 01:12:56
tags: [Programming]

---

这些内容是在C++考试前突击复习整理出来的，一些零碎知识、代码和容易混淆、忘记的知识。

这些对编程实践可能并没有什么用，但对于考试的填空选择可能用处不少，因此写出来供我自己和大家参考。

因为时间原因，有些内容还没有经过编程证实，如果有错误还请联系我，我会加以改正。

另外我打算在近期（期末考试结束后？）启用博客的评论系统，到时候方便交流啦～~~咕咕咕？~~

<!--more-->
## 零碎知识
* 面向对象程序设计思想的主要特征有：
	1. 封装性
	2. 多态性
	3. 继承性


* C++中用户自定义的数据类型：
	1. `class`类
	2. `struct`结构体
	3. `union`共用体
	4. `enum`枚举


* 类中不做特别声明的话，所有成员都是`private`的。
* 构造函数的作用：
	1. 给创建的对象建立一个标识符
	2. 为对象数据成员开辟内存空间
	3. **完成对象数据成员的初始化**


* 析构函数前面的`~`是**位取反运算符**。
* 析构函数的作用是在对象被系统释放之前做一些内存清理工作~~被宰之前先拔毛~~。
* 静态成员函数没有`this`指针，即不能通过其访问本对象的成员，所以静态成员函数一般用来访问静态成员而非非静态成员。
* 两种多态性：
	1. 静态多态性：靠**函数重载**实现，在编译时即已确定调用的是哪个函数。
	2. 动态多态性：靠**虚函数**实现，不在编译时确定调用的函数，运行时才确定。


* 不能重载的运算符：
	* `.`成员访问运算符
	* `*`成员指针访问运算符
	* `::`域运算符
	* `sizeof`长度运算符
	* `? :`条件运算符


* 不需要用户重载的运算符：`=`和`&`。
* 重载运算符的规则：
	1. 不允许定义新运算符
	2. 不能改变运算对象（操作数）个数
	3. 不能改变优先级
	4. 不能改变结合性
	5. 不能有默认参数


* 派生时不做特殊说明则继承方式是`private`。

### 代码
* 函数模板

	```cpp
	template<typename T1, typename T2>;
	template<class C1, class C2>;
	```
	相当于生成了几个在使用时才确定的类，可以让函数在调用时确定某些参数的类型，如

	```cpp
	int func(T1 a, T2 b);
	```
	这个`func`中的`T1`和`T2`会随传入的参数类型的改变而改变，变成它**第一个**遇到的参数的类，然后在这个函数中的所有`T1`和`T2`都会变成相应的类，且只在这个函数的**此次调用**中生效。  
	同时需要注意的是隐藏的类型转换（如`char`可以自动转换成`int`的这种）不起作用，如果参数表里用的都是`T`而传入的是两个不同类型的参数，那就会错，此时在调用时应该用强制类型转换或直接舍弃函数模板。
* 拷贝构造函数

	```cpp
	A(A &a);
	```

* 运算符重载  
	* 用成员函数的方式  

		```cpp
		A operator + (A &a1, A &a2);
		```
		因为作为成员函数可以用`this`访问本对象的成员，因此可以省略参数`A &a1`。

	* 用友元函数的方式

		```cpp
		friend A operator + (A &a1, A &a2);
		```
		不能省略参数`A &a1`。

	流插入运算符`>>`的第一个参数是`istream &`；流提取运算符`<<`的第一个参数是`ostream &`。

## 继承与派生
### 虚继承和虚基类
```cpp
class B: virtual public A
```
在多重继承时避免二义性问题。

基类`A`中的成员变量`a`如果派生出`B`、`C`，再由`B`、`C`派生出`D`的时候在`D`中访问`a`会出现二义性问题。  
使用虚继承，即声明从`A`继承的`a`还是`A`的`a`，而不会变成`B`的`a`，从而在派生`D`的时候让`D`继承`A`的`a`，从而不会引起二义性问题。

### 赋值兼容规则
1. 派生类对象可以赋值给基类对象。
2. 反之不行。
3. 基类对象的指针可指向派生类对象，也就是派生类的指针可以赋值给基类对象的指针，**但在有多重继承时基类指针只能调用本基类在派生类对象中的成员，而无法调用其它基类在派生类对象中的成员**。
4. 派生类对象可以初始化基类型的引用，**但在有多重继承时基类引用只能调用本基类在派生类对象中的成员，而无法调用其它基类在派生类对象中的成员**。

## 多态性与虚函数
### 虚函数
```cpp
virtual int func(int);
```
1. 在基类中声明一个成员函数`func`为虚函数。
2. 在派生类中声明一个同名成员函数`func`。
3. 用一个基类指针调用`func`时即可调用派生类的`func`；如果不将基类中的`func`声明为虚函数，则基类指针调用的是基类的`func`。

注意，构造函数**不能**声明为虚函数。

#### 纯虚函数
```cpp
virtual int func(int) const = 0;
```
只有声明，没有函数体，不能被调用，相当于在基类中不存在，**也因此包含纯虚函数的类无法生成对象**。声明它是为了说明在派生类中会出现且被定义，然后就可以用虚函数的方法用基类指针调用派生类的`func`了。

#### 抽象类
唯一的作用是拿来派生，并不会被用来生成对象~~只负责下蛋而不会用来吃（不恰当的比喻）~~，凡是包含虚函数的类都是抽象类，**因为包含纯虚函数的类无法生成对象**。

可以作为函数的参数类型，例如：  
现有抽象类`A`  
可以有这样一个函数：

```cpp
int func(A &a);
```
体现了动态多态性。
