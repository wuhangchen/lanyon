---
layout: post
title: Argument-dependent-name-lookup（参数依赖查找)下补充 
comments: true
category: summary
tags: [C++  grammar ]  
---   
### Namespace Qualification or ADL  
Many programmers do not want to get into the complicated rules of how a compiler picks an overload or runs into ambiguities,They qualify the namespace of the called function and Know exactly which function overload is selected(assuming the overloads in that namespace are not ambiguous within the overload resolution ),We do not blame them;the name lookup is anything but trivial.  
When we plan to write good generic software containing function and class templates instantiatable with many types,we should consider ADL,we will demonstrate this with a very popular performance bug (especially in c++03)that many programmer have run into ,the standard library contains a function template called swap ,It swaps the content of two object of same type,the old default implementation used copies and a temporary;  
```c++  
	template<class T>  
	inline void copy(T &x,T &y)  
	{	
		T tmp(x);x=y;y=tmp;
	} 
```   
it works for all type with copy constructor and assignment,so far ,so good,say we have two vectors ,each containing 1GB of data,then we have to copy 3GB and also need a spare gigabyte of memory when we use the default implementation, or we do something smarter ,we switch the pointers referring to the data and the size information.  
```c++  
	template <class value> 
	class vector
	{
		friend inline void swap(vector &x,vector &y)
		{std::swap(x.my_size,y.my_size);std::swap(x.data,y.data);}
	private:  
		unsigned my_size;
		value *data;
	};  
``` 
Note that this example contains contains an inline-friend function,This declares a free function which is a friend of the contained class,apparently ,this is shorter than separate friend and function declaration.  
Assuming we have to swap data of a parametric type in some generic function:  
```c++  
	template<typename T,typename U>  
	inline void some_function(T &x,T&y,const U& z,int i) 
	{
		std::swap(x,y);//can be expensive
	}  
```  
we play it safe and used the standard swap functgion which work with all copyable types ,but we copied 3GB,it would be much faster and memory-efficient to use ore implement that only switches the pointers.This can be achieved with a small changes in a generic manner :  
```c++  
	template<typename T,typename U>  
	inline void some_function(T &x,T&y,const U& z,int i) 
	{
		using std::swap;
		swap(x,y);//involves ADL.
	}  
```   
with this implementation ,both swap overloads are candiates but the one in our class is prioritized by overload resolution as its argument type is more specific than that of the standard implementaion,More generaly,any implementation for a user type is more specific than std::swap,In fact ,std::swap is already overloaded ofr standard containers for the same reason ,this is a general pattern.  
As an addendum to the default swap implementation :since c++11,the default is to move the values between the two arguments and the temporary;  
```c++  
	template<typename T>  
	inline void swap(T& x,T& y)
	{
		T tmp(move(x));
		x=move(y);  
		y=move(tmp);
	} 
```  
as a result types without userf-defined swap can be swapped efficiently when they provide a fast move constructor and assignment ,Only types without user implementation and move support are finaly copied.