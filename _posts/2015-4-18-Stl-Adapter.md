---
layout : post
time : 2015-4-18
title : SGI Stl的Adapter的赏析
category : Stl
keywrods : Stl, Adapter 
tags : Stl
description : 对SGI Stl中各类适配器的源码分析以及模仿实现
---

## 前言

上次我们讲到了**Allocator**，今天我们来谈一谈和它同姓的一个小伙伴：**Adapter**．它的中文翻译是**适配器**,通过这个翻译也许你就能猜测到Adapter不会想我们之后会谈到的vector,deque,list....这种直接的容器类型那样，事实上Allocator确实有它特殊之处．我想看完本章之后，你就会更加迫不及待的进入stl个中容器类的背后来一窥究竟．

## Stack

Stack是一种**first in last out**的数据结构，用户只能在其一端（通常称为top）进行数据的存取．因为它支持的功能相对简单（例如相对与以后要介绍到的big buddy deque而言），所以我们其实不需要额外的去实现一套stack，只需要使用vector，deque．．．的部分功能来实现就可以．

{% highlight C++ %}
template<typename Tp,typename SequenceContainter>
class stack {
		SequenceContainter c;
	public:
		......
		void push(const Tp& v) { c.push_back(v); }
		void pop() { c.pop_back(); }
};
{% endhighlight %}

上面是stack的大概架构，从中我们不难发现，stack的操作都是通过调用内部的其他顺序容器对象来实现的．这种"用他人之力，来成我之事"的类就被称为**Adatper**.

## Heap

Stl中的heap有一点特殊，它并不是存储特定数据的结构，而是依托与其他顺序容器的方法．通过heap中make_heap,push_heap,pop_heap,sort_heap的方法，用户可以对指定顺序容器特定区间内的值做相应调整．我们来具体看一下工作流程．

### make_heap

首先我们来看建堆函数．make_heap的功能是将顺序容器中[begin,end)区间内的值做调整，使得其满足最大堆／最小堆的限制.

该方法从堆的倒数第二层中，最右节点idx开始，迭代对[idx -> begin]内全部元素执行__fill_hole_with_child操作. __fill_hole_with_child这个操作能将idx放置到其子树中合适的位置，并且顺带调整其子树．

### push_heap

将需要压入的数据value放置在[begin,end)区间的尾部，然后迭代的调整value与其父节点的位置．

### pop_heap

首先记录尾节点的数据value，并将首节点放置在尾节点上．然后将value放置在首节点的位置begin.　

现在我们的任务就是调整[beign,end-1)内数据（想想为什么时end-1,而不是end呢？）．我们可以使用__fill_hole_with_child来填充begin，之后调用push_heap来压入value．

### sort_heap

迭代的调用pop_heap来获取堆顶元素．



