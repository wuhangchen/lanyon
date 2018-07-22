---
layout: post
title: man_select_poll_function 
comments: true
category: summary
tags: [unix network programming ]
---

```c++
    int select(int maxfdp1,fd_set *readset,fd_set *writeset,fd_set *execptset,const struct timeval *timeout);
```

    this function allows the process to instruct the kernel to wait for any one of multiple events to occur and to 
wake up the process only when one or more of these events occurs or when a specified amount of time has passed.

    we tell the kernel what descriptor we are interated in (for reading,writing,or an exception condition)and how long to wait.

1. the final argument ,which tells the kernel how long to wait for one of the specified descriptors to be become ready,a stimeval 
structure specifies the number of seconds and microseconds.
```c++
    struct timeval{
        long tv_sev;
        long tv_userc;
    };
```
    there are three possibilities:
    - wait forever,return only when one of the specified descriptors is ready for io,for this,we specify the timeout argument as a null pointer.
    - wait up to a fixed amount of time,return when one of the specified descriptors is ready for io.but do not wait beyond the number of seconds and microseconds specified in the timeval sturcture pointer to by the timeout argument.
    - do not wait at all,return immediately after checking the descriptors ,this is called polling ,to specifying this,the timeout argument must point to a timeval structure and the timervalue(the number of seconds and microseconds specified by the structure)must be 0.

    the const qualifier on the timeout argument means it is not modified by select on return .

2. the three middle arguments ,readset,writeset,and exceptset,specify the descriptors that we want the kernel to test for reading,writing ,and exception conditions,there are two exception conditions currently supported:
    
    -  it is improtant to initialize the set ,since unpredictable results can occur if the set is allocated as an automatic variable and not initalized,
    - any of the middle three argument to select readset,writeset,or exceptset,can be specified as a null pointer if we are not interested in that condition.

3. the maxfdp1 argument specifies the number of descriptors to be tested,its value is the maximum descriptor to be tested puls one (hence our name of maxfdp1)

    the two most common programming errors when using the select are to forget to add one to the largest descriptors number and to forget that the 
descriptor sets are value-result arguments,the second error reults in select being called with abit set to 0 in the descritpor set,when we think that bit is 1.
4. the return value from this function indicaties the total number of bits that are ready across all the descriptor sets,if the timer value expires before any of the descriptors are ready,a value of 0 returned,a return value of -1 indicated an error.



```c++
    int shutdown(int sockfd,int howto);
```
5. the normal way to terminate a network connection is to call the close function,but there are two limitations with close that can be avoided with shutdown.
    - close decrements the descriptor's reference count and closes the socket only if the count readches 0.with the shutdown,we can initial tcp's normal 
    connection termination sequence,regardless the reference count.
    - close terminates both directions of data transfer,reading and writing.since tcp connection is full-duplex,there are times when we want to tell the other end that we have finished sending ,even though that end might have more data to send us.
    
6. the action of the function depends on the valueof the howto argument.
    - SHUT_RD the read half the connection is closed,
    - SHET_WR the write half of the connecton is closed.
    - SHUT_RDWR the read half and the write half of the connetion are both closed,this is equivalent to calling shutdown twice :first with SHUT_RD and then with SHUT_WR.


```C++
    int poll(struct pollfd *fdarray,unsigned long nfds,int timeout);
```
7. allowing poll to work with any descriptor poll provides functionality that is similar to select,but poll provides additional information then dealing with streams devices.

    - the first argument is a pointer to the first element of an array of structures,each element of the array is a pollfd structure that specifies that specifies the conditions 
    to be tested for a fiven descriptor .
    ```c++
        struct pollfd{
            int fd;
            short events;
            short revents;
        };
    ```
    - the timeout argument specifies how long the function is to wait before returning .


