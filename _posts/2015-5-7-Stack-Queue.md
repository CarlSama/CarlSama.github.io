---
layout : post
time : 2015-5-7
title : Stack and Queue
category : Algorithm
keywrods : Algorithm, CareerCup 
tags : CareerCup 
description : Reading note of < Cracking the code interview >
---

##

Awesome ~

## Design a stack, push,pop and min all operate in O(1) time

1.	record current min element in each node of stack.If we have large stack, we waste a lot of space.(local min)

2.	Use a status stack to keep track of minimum elem from current position to the bottom of data stack. When doing pop, just compare the status stack and data stack to decide wether we need to pop the status stack. 

## Write a program to sort a stack into ascending order.

We need to use additional stack to help arrange data. When sorting, dealing with data with the help of two stack.

## Use two stack to simulate a queue

Ask what method do interview want ?

{% highlight C++ %}
template<typename T>
class queue {
		stack<T> s1,s2;
	public:
		int size() { return s1.size() + s2.size();}
		void push(T val) { s1.push(val); }
		T pop() {
			if(s2.empty()) {
				while(!s1.empty()){
					s2.push(s1.pop());
				}
			}
			return s2.pop();
		}
		T top() {
			if(s2.empty()){
				while(s1.empty()) {
					s2.push(s1.pop());
				}
			return s2.top();
		}
{% endhighlight %}

## Use two queue to simulate a stack

