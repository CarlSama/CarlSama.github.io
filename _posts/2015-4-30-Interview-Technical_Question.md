---
layout : post
time : 2015-4-30
title : Five step to a technical question
category : Interview
keywrods : Interview, Technical Question, CareerCup
tags : CareerCup
description : Reading note of < Cracking the code interview >
---

## Steps to technical question

1.	ask your interviewer questions to resolve ambiguity.

2.	design algorithm.

3.	write qsedu-code first.

4.	write code, not too fast and not too slow.

5.	test code and be carefully.

### Step 1 : Ask Questions

Good questins may like this :

1.	What are the data types ?

2.	How much data ?

3.	What assumption do you need to solve the questions ?

4.	Who is the user ?

#### Case

"Design an algorithm to sort list" > "What sort of list ? array ? linked list ?" "array" 
"What does the array holds ? numbers ? characters ? strings ?" "numbers" 
"Are they number integer ?" "Yes"
"Where did the number come from ?" "Age"
"How many numbers ?" "Millions"

That is sort array contain millions integer range from 0 to 130

### Step 5 : Test

Extremen Case : 0, negative, null, maximum, minumum ...

User error : zero or negative or invalid input

General case : normal case

Tips : 
1.	Consider testing while you are writing the code not at end.

2.	When you find a mistake, caruful think through why the bug is occuring, not just make random change. :)


## Five Algorithm Approaches

### EXAMPLIFY

### PATTERN MATCHING

find a loop invarient

### simplify && generalize

### Build on Basic Case

### Data structure brainstorm

Numbers are randomly generated and stored into an array. How can you keep track of medians ( be careful of even and odd )

1.	linked list ? Linked list performs very bad with accessing and sorting

2.	array ? you may need to keep the generated number into sorted order

3.	Binary tree ? Binary does fairly well with ordering. If the binary tree is perferct balance, the root might just be the median. However, is the num of generated number is even, the median should be the xxx. You may need to find both elem.

4.	Heap ? we can use two heap to keep track of the bigger half of the generated number and the smaller half. Of course, the bigger half should be organized as min heap and hte smaller half should be max heap.
