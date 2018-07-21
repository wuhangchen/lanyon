---
layout: post
title: linux-kernel-double-list 
comments: true
category: summary
tags: [ linux kernel  ]
---

#### linux-kernel-double-list 


常常声明在如下头文件中 `/usr/src/linux/include/types.h`.
```c
    struct list_head{   
        struct list_head *next,*prev;
    };
```
而常常实现在如下头文件 `/usr/src/linux/include/list.h`.

```c
    #define LIST_HEAD_INIT(name) { &(name), &(name) }
           
    #define LIST_HEAD(name) \   
    struct list_head name = LIST_HEAD_INIT(name)
//上面的代码是宏的形式来初始化一个新链表，它申明类型为`list_head`的新变量`name`,该变量作为新链表头的一个占位符，是一个亚元素，同时还会初始化`list_head`数据结构的`prev``next`字段，让他们指向`name`变量本身。

```
//下面为一个静态函数初始化一个链表。
```c
    static inline void INIT_LIST_HEAD(struct list_head *list)
    {
        list->prev = list;
        list->next = list;
    }

```
`list_add(n,p)`，将n指向的元素插入p指向的特定元素之后:
```c
    static inline void list_add(struct list_head *new, struct list_head *head)
    {
        __list_add(new, head, head->next); 
//转调用第三个函数来完成插入功能
    }

    static inline __list_add(struct list_head *new,
                                struct list_head *prev,
                                struct list_head *next)
    {
        next->prev = new;
        new->next = next;
        new->prev = prev;
        next->next = new;
    }
``` 
`list_add_tail(n,p)`，把n指向的元素插入到p所指向的元素之前(前插):
```c++
    static inline void list_add_tail(struct list_head *new,struct list_head *head)
    {
        __list_add_(new,head->prev,head);
    }
```
`list_del(p)`删除p所指向的元素：
`list_empty(p)`检测第一个元素的地址p指向的链表是否为空
`list_for_each(p,h)`对表头地址h指定的链表进行扫描，在每次循环时，通过p返回指向链表元素的list_head结构的指针
`list_for_each_entry(p,h,m)`与list_for_each类似，但是返回了包含了`list_head`结构的数据结构的地址，而不是list_head结构的本身的地址。

### 总结
内核还支持另外一种双向链表名为hlist_head，这种散列表的实现我们将在后面来剖析。





