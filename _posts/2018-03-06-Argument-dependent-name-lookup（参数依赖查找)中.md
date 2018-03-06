---
layout: post
title: Argument-dependent-name-lookup（参数依赖查找)中
comments: true
category: summary
tags: [C++  grammar ]  
---  
## Interfaces
While ADL makes it practical for functions defined outside of a class to behave as if they were part of the interface of that class, it makes namespaces less strict and so can require the use of fully qualified names when they would not otherwise be needed. For example, the C++ standard library makes extensive use of unqualified calls to std::swap to swap two values. The idea is that then one can define an own version of std::swap in one's own namespace and it will be used within the standard library algorithms. In other words, the behavior of  
    `std::swap(a, b);`  
may or may not be the same as the behavior of  
```c++
	using std::swap;
	swap(a, b);  
```  
(where a and b are of type N::A) because if N::swap(N::A&, N::A&) exists, the second of the above examples will call it while the first will not. Furthermore, if for some reason both N::swap(N::A&, N::A&) and std::swap(N::A&, N::A&) are defined, then the first example will call std::swap(N::A&, N::A&) but the second will not compile because swap(a, b) would be ambiguous.
In general, over-dependence on ADL can lead to semantic problems. If one library, L1, expects unqualified calls to foo(T) to have one meaning and another library, L2 expects it to have another, then namespaces lose their utility. If, however, L1 expects L1::foo(T) to have one meaning and L2 does likewise, then there is no conflict, but calls to foo(T) would have to be fully qualified (i.e. L1::foo(x) as opposed to using L1::foo; foo(x);) lest ADL get in the way.