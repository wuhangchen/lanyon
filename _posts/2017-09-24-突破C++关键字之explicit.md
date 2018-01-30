---
layout: post
title: 突破C++关键字之explicit   
comments: true
category: summary
tags: [C++ keyword ]  
---  
## 意象不到的结果  
当你写好构造函数的的时候，编译器很可能会出现将你所写的代码进行你意象不到的转换，这一切的来源在于 ** 类的构造函数能够进行隐式转换！**  
例子一：  
```C++  
	class String
	{
		public:
		/*explicit*/ String(const char* data);//顶层const
		...
	};

```  
例如，函数f()有一个String类型的参数，如果String 的构造函数如上所示就会产生意象不到的结果：可以通过字面字符串
隐式的转换为String对象而使的调用f("helloworld")生效。当然，这肯定不是我们想要的。  
例子二：  
```C++  
	class Stack
	{
		public:
		/*explicit*/ Stack(int size);
	};
	Stack s;
	s = 40;
```  
构造函数有能力将一个int类型自动转换为Stack。一旦这种情况发生，会将40转换为含有40个元素的Stack，然后拷贝给s,那么结局肯定不是我们想要的。  
## 如何解决  
对与单参数的构造函数，很有可能会发生我们想不到的隐式转换，如果我们不是想要它进行隐式转换，那么将**构造函数的申明为explicit加以阻止**，是一个不错的方法。  
关键字explicit只对单实参的构造函数有效，需要多个实参的构造函数不能用于执行隐式转换，同时，只需要在类内申明时使用关键字即可，在外部定义的时候就不在需要了。  
## explicit构造函数只能用于直接初始化
简单辨析直接初始化与拷贝初始化：  
```C++  
	string s("helloworld");//直接初始化
	string s2 = "helloworld"；//拷贝初始化  
```
发生隐式转化的常常发生在拷贝初始化中，但是申明为explicit构造函数后，使用拷贝初始化将发生错误。只能使用直接初始化。  
## 转换为显式的使用构造函数  
虽然编译器不会将explicit的构造函数用于隐式转换过程，但是我们可以使用这样的构造函数显式强制转换.实质上就是构造出一个临时对象来进性操作。  
## 标准库中含有显式构造函数的类  
- 接受一个单参数的const char *的string构造函数。既有含有explicit，还有不含有explicit的参数。部分源码如下：
```C++
	namespace std{
	typedef basic_string<char> string;
	...
	basic_string();
	...
	explicit basic_string(const allocator_type& _A1);
	...
	basic_string(const basic_string& right);
	....
	basic_string(basic_string&& right);
	}//来源于MSDN文档code.
```  
- 接受一个容量参数的vector的构造函数是explicit的，部分代码如下：  
```C++  
	
	explicit vector(size_type n) { fill_initialize(n, T()); }  
  
    vector(const vector<T, Alloc>& x)  
    {  
        start = allocate_and_copy(x.end() - x.begin(), x.begin(), x.end());  
        finish = start + (x.end() - x.begin());  
        end_of_storage = finish;  
    }  
	...
```  
看的这些STL的源代码真心给力，太不要让人着迷（逃。


