---
layout: post
title:解析stl之hashtable
comments: true
category: summary
tags: [stl ]  
--- 
#### hashtable 


###### 为何出现hashtable 
后面我们会讲诉一种非常重要的数据结构，红黑树，它成为了stl 中`set` 和`map`,`rbtree`在树形结构保持着平衡，也同时在
效率表现和实现的复杂度上也保持着相当的平衡，所以运用极广。 

2叉搜索树也同样具有对数平均时间的表现，但是这样的表现构造在一个假设上：输入数据有足够的随机性。所以导致了`hashtable`的出现。 
同样这种结构在插入、删除、搜寻等操作上具有`常数平均时间`的表现，而且这种表现是以统计为基础的，不需要依赖输入元素的随机性。 


###### hashtable 的数据结构 
简而言之：`vector` + `slist`;
这种`slist`并非stl中`list`或者是`slist`，而是自己维护的一个单链表。



