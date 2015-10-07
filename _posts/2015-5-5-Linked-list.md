---
layout : post
time : 2015-5-5
title : Linked list
category : Algorithm
keywrods : Algorithm, CareerCup 
tags : CareerCup 
description : Reading note of < Cracking the code interview >
---

## Remove duplicate from unsorted linked list without using temporary buffer.

single linked list or doubly linked list ?

1.	hash would be good, but can i use that ?
	If I can use hash, how many would the character would be ? This may helps me using vector as hashtable.

2.	just compare, cost O(n^2) time and no extra space.

I have a bug here ! When runs into adjacent ?


{% highlight C++ %}
bool hasUniqueCharacters(string str) {
	int exist = 0;
	for(int i =0 ;i<str.size();++i) {
		int pos = str[i] - 'a';
		if(exist & ( 1 << pos )) {
				return false;
		}
		exist |= (1 << pos);
	}
}
{% endhighlight %}

## Find the nth to last of linked list(倒数第n个)

1.	We may compare Iteration and Recurssion here. Recurssion may cause stack overflow.

2.	It is needed to ask the length of n, can I stored it into a int type ?

Iteration : Use two pointer.

Recurrsion :
{% highlight C++ %}
Node* find(Node* head,int n,int level) {
	Node* res = 0;
	if(head->next)
		res = find(head->next,n,level);
	++level;
	if(level == n) return head;
	else 
		return res;
}
Node* findNthToLast(Node* head,int n) {
	if(0==head || n < 0) return  NULL;
	int level = 0;
	return find(head,n,level);
}
{% endhighlight %}


## Delete the mid node of linked list

I should disscuss the copy-and-delete algorithm would not fit if the mid node is the last node(mid->next == NULL)
