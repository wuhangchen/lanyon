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

1)`hashtable`的节点定义如下：
```c++ 
    template<typename Value>
    struct __hashtable_node
    {
        __hashtable_node* next;
        Value val;
    };
```
2)`hashtable`的迭代器定义如下： 
```c++ 
//如果没有对stl中的哈希表更高的功力，那么使用起来可能有一些问题。
//下面为迭代器的定义，里面很多类型定义
    template<class Value,class key,class HashFcn,class ExtractKey,class EqualKey,class Alloc>
    struct __hashtable_const_iterator {
        typedef hashtable<Value, Key, HashFcn, ExtractKey, EqualKey, Alloc>
                 hashtable;
        typedef __hashtable_iterator<Value, Key, HashFcn, 
                               ExtractKey, EqualKey, Alloc>
                iterator;
        typedef __hashtable_const_iterator<Value, Key, HashFcn, 
                                     ExtractKey, EqualKey, Alloc>
                const_iterator;
//节点的定义
        typedef __hashtable_node<Value> node;
//stl迭代器的设计规范
        typedef forward_iterator_tag iterator_category;
        typedef Value value_type;
        typedef ptrdiff_t difference_type;
        typedef size_t size_type;
        typedef const Value& reference;
        typedef const Value* pointer;
//2个数据元素
        const node* cur;
        const hashtable* ht;
//构造函数
        __hashtable_const_iterator(const node* n, const hashtable* tab)
            : cur(n), ht(tab) {}
        __hashtable_const_iterator() {}
        __hashtable_const_iterator(const iterator& it) : cur(it.cur), ht(it.ht) {}
//以下函数为 模仿底层的运算符的设计。没有什么太大的困难，
//注意重载了前置型的operator++,以及operator++ 后置型需要格外的注意，里面可能并不是原声的运算符，而是已经经过
//重载的运算。这里需要语法上的知识。
        reference operator*() const { return cur->val; }
#ifndef __SGI_STL_NO_ARROW_OPERATOR
        pointer operator->() const { return &(operator*()); }
#endif /* __SGI_STL_NO_ARROW_OPERATOR */
        const_iterator& operator++();
        const_iterator operator++(int);
        bool operator==(const const_iterator& it) const { return cur == it.cur; }
        bool operator!=(const const_iterator& it) const { return cur != it.cur; }
    };

```
`hashtable`迭代器必须永远维系与整个`bucket vector`的关系，并记录目前所指向的节点。其前进的操作
是首先尝试从目前所指向的节点出发，前进一个位置，由于节点被安置于`list`之内，利用节点的
`next`指针可以轻易的达成操作，但是试想一下目前节点正好在`list`的尾端，那么就必须跳至下一个
`bucket`身上，那正是指向下一个`list`的头部节点。
```c++
//类外定义`operator++`函数，可以看出，前置型的++返回类型为引用，
    template <class V, class K, class HF, class ExK, class EqK, class A>
    __hashtable_const_iterator<V, K, HF, ExK, EqK, A>&
    __hashtable_const_iterator<V, K, HF, ExK, EqK, A>::operator++()
    {
//增加一个中间层
        const node* old = cur;
        cur = cur->next;
//判断是否还是同一个`list`里面，若进入if语句，则表明当前在最后节点。
        if (!cur) {
//利用ht指针调用hashtable中成员函数bkt_num来计算在哪一个bucket身上。
            size_type bucket = ht->bkt_num(old->val);
//判断是否溢出vector里。
            while (!cur && ++bucket < ht->buckets.size())
                cur = ht->buckets[bucket];
        }
        return *this;
    }
//后置型的++倒是需要格外留意，
//套路其实不难，保存中间层，递增当前对象，然后返回中间层。
//但是里面的各种调用可能不是你所看到的那样简单的函数调用，很大可能是已经经过重载的运算符。   
    template<class V,class K,class HF,class ExK,class EqK,class A>
    inline 
    __hashtable_iterator<V, K, HF, ExK, EqK, A>
    __hashtable_iterator<V, K, HF, ExK, EqK, A>::operator++(int)
    {
        iterator tmp = *this;
//转调用前置型的递增运算符。
        ++*this;
        return tmp;
    }
```

这便是hashtable中节点定义以及迭代器的设计，下篇博客我们将讲解其中最为重要的hashtable
的数据结构，以及内存构造，插入等设计。
