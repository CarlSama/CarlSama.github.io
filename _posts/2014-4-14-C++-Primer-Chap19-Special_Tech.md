---                                                                                 
layout : post
time : 2014-4-14
title : C++ Primer -- Chap019
category : C++ 
keywrods : C++, C++ Primer
tags : C++ 
description : my reading notes about C++ Primer chap 19
---

##

new表达式执行的过程:
1. 调用operator new 的标准库函数,分配原始空间
2. 运行构造函数并传入初始值
3. 构造完成后返回指向该对象的指针

delete表达式执行的过程:
1. 执行析构函数
2. 调用operator delete 的标准库函数释放内存空间


