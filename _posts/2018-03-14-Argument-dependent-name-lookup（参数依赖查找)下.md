---
layout: post
title: Argument-dependent-name-lookup（参数依赖查找)下
comments: true
category: summary
tags: [C++  grammar ]  
---  
we continue study the concept of Argument-dependent-name-lookup ,which expands the search of function names to the namespaces qualifieation for function .  
```C++  
	namespace rocketscience{
	struct matrix{};
	void initialized(matrix &A){...}
	matrix operator+(const matrix &A,const matrix &B)  
	{
		matrix C;
		initialized(C);//not qualified ,same namespaces  
		add(A,B,C);  
		return C;
	}
}  
```  
everytime we use the function initialize ,we can omit the qualification for all classes in namespace rockscience.  
the ADLmechanism searches functions only in the namespaces of the arguments type declarations but not in their repective parents  namespaces.  
when we import a name within another namespace,the functions in that namespaces are not subject to ADL either.  
at the same time ,Relying the ADL only for selecting the right overload has its limitations ,when we use a third-party library,we may find functions and operations that we also implemented in our namespaces,such ambiguities can be reduced (but not entirely avoided )by using only single functions instead of entire namespaces.  
The probability of ambiguities rises further with multi-argument functions ,especially when parameter type come from different namespaces . 
but unfortunately,explicit template instantiation dows not work with ADL,whenever template arguments are explicitly declared in the function call,the function name is not souught in the namespaces of the argument,  
which function overload is called depends on the so-for discussed rules on :  
- Namespaces nesting and qualification.  
- Name hiding .  
- ADL.  
- Overload resolution.  
