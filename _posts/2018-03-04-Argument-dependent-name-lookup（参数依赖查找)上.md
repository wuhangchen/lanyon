---
layout: post
title: Argument-dependent-name-lookup（参数依赖查找)上 
comments: true
category: summary
tags: [C++  grammar ]  
---
In the C++ programming language, argument-dependent lookup (ADL), or argument-dependent name lookup, applies to the lookup of an unqualified function name depending on the types of the arguments given to the function call.   

During argument-dependent lookup, other namespaces not considered during normal lookup may be searched where the set of namespaces to be searched depends on the types of the function arguments. Specifically, the set of declarations discovered during the ADL process, and considered for resolution of the function name, is the union of the declarations found by normal lookup with the declarations found by looking in the set of namespaces associated with the types of the function arguments.

## Example
```c++  
	namespace NS 
	{
   		class A {};
   		void f( A &a, int i) {}
	}
	int main() 
	{
   		NS::A a;
   		f( a, 0 );    //calls NS::f
	}
```  
A common pattern in the C++ Standard Library is to declare overloaded operators that will be found in this manner. For example, this simple Hello World program would not compile if it weren't for ADL:  
```c++  
	#include <iostream>
	#include <string>

	int main()
	{
   	 	std::string str = "hello world";
    	std::cout << str;
	}  
```  
Using << is equivalent to calling operator<< without the std:: qualifier. However, in this case, the overload of operator<< that works for string is in the std namespace, so ADL is required for it to be used.  
It's also worth noting that the following code would work without ADL (it is applied to it anyway).  
```c++
	#include <iostream>

	int main()
	{
   	 	std::cout << 5;
	}
```  
It works because the output operator for integers is a member function of the std::ostream class, which is the type of cout. Thus, the compiler interprets this statement as  
    `std::cout.operator<<(5) ;`  
which it can resolve during normal lookup. However, consider that e.g. the const char * overloaded operator<< is a non-member function in the std namespace and, thus, requires ADL for a correct lookup:  
```c++
	/* will print the provided char string as expected using ADL derived from the argument type std::cout */
	operator<<(std::cout, "Hi there")

	/* calls a ostream member function of the operator<< taking a void const*, which will print the address of the provided char string instead of the content of the char string */ 
	std::cout.operator<<("Hi there")
```  
The in the std namespace overloaded non-member operator<< function to handle strings is another example:
    `std::cout << str`  
Without ADL the compiler would have thrown an error stating it could not find operator<< as the statement doesn't explicitly specify that it is found in the std namespace.