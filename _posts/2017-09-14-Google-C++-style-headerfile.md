---
layout: post
title: Google-C++-style-headerfile
---

## 前言

本想继续戏说关键字const以及Cpp的内存布局这个系列的博客，在昨天偶然发现[Google开源项目风格指南](http://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/headers/)。在看了一遍之后，发现对以前学的code风格居然没有早点看到这份
指南。于是，先放出自己总结的一点，后续在更继续更。关键不在于学习，而是运用到所写的code上去，这才是最为关键的。

## 头文件


总结:

一般来说通常每一个``.c``文件都含有一个对应的``.h``文件，同时将类的申明包含在头文件。



## define保护


总结:

 所有的头文件都应该使用``#define``来防止头文件被多重包含，在编译时，不然编译时会导致error。
 命名格式：``<PROJECT>_<PATH>_<FILE>_H``。
 

 例如举例如下：项目``foo``中的头文件``foo/src/bar/baz.h``就应该按照如下方式：
 
```C++
  
     #ifndef FOO_BAR_BAZ_H
     #define FOO_BAR_BAZ_H
     ...//类的申明等
     #endif //FOO_BAR_BAZ_H
     
```

## 前置申明：

  
 总结:
  
 尽可能避免使用前置申明，使用``#include``包含的头文件。
      
   
 
 
 ##  内联函数

 总结:
 
 只有一般当函数只有10行或者更少才将其定义为内联函数。
 
 

 
 
 
 ##  include的路径
 
 总结:
  
  使用标准的头文件包含的顺序可以增强可读性顺序如下：相关头文件、C库、C++库、其他库。
  项目内的头文件应该按照项目的源代码目录树结构排列。避免使用Linux特殊的快捷目录。
  

  举例来说, google-awesome-project/src/foo/internal/fooserver.cc 的包含次序如下:
  
  ```C++
      #include "foo/public/fooserver.h" // 优先位置

      #include <sys/types.h>      //C系统头文件
      #include <unistd.h>

      #include <hash_map>         //C++库头文件
      #include <vector>

      #include "base/basictypes.h"      //项目头文件
      #include "base/commandlineflags.h"
      #include "foo/public/bar.h"
 ```
  
