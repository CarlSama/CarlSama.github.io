---
layout : post
time : 2015-5-1
title : Array and String 
category : Algorithm
keywrods : Algorithm, CareerCup 
tags : CareerCup 
description : Reading note of < Cracking the code interview >
---

## Determine if a string has all unique characters 

1.	Compare each character with all the characters behind it, this would takes O(n^2) time and no extra space.

2.	Is the input string pass by reference ? Can we modified the input string ? We may sort the string first and check the adjacent characters. This may takes O(nlgn) time, be careful that some sort algorithm may use extra space.

3.	What is the character set of input string ? We can use array as hash table. This would tasks O(n) time and O(n) space.

4.	Use bit vector would save space. If we can assume the string only containes character ranging from 'a' to 'z', we may just need to use a INT type.


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

## Remove duplicate character from string without using additional buffer.

Always remember that analysis the problem carefully first without using memory instead ! This is not remove adjacent duplicate character !

1.	No additional memory.

Test Case : 
{% highlight C++ %}
1.	null string ; 
2.	continus duplicate ;
3.	un-continuous duplicate ;
4.	all duplicate ;
5.	no duplicate ;
6.	empty  string ;
{% endhighlight %}

2.	Does the additional buffer means constant size of additional memory ?

We can use vector(hash table) to help determine check, This would cost constant memory.
{% highlight C++ %}
vector<bool> exist(256, false);
{% endhighlight %}


## Decide two strings are anagrams or not 

**ANAGRAM** means two string are constructed with same number of characters

**PALINDROMIC** means a string is identical from left to right and right to left.

1.	We can sort and compare
{% highlight C++ %}
	return sort(a) == sort(b);
{% endhighlight %}

2.	We may use vector as hash table to calculate the appearence of each characters.

## Replace all space in a string with '%20'

1.	Can I use extra space ? count the space num and make new storage, traverse from left to right and store.

2.	I have to do in place , which means str have enough space behind it ? count and traverse from back.

## Rotate M*N matrix in place

use colume and row to find clue

## If matrix[i][j] is 0, set the entire colume & row to 0

record and update later

## Check str1 is rotation of str2

concatenate str1 with itself to get sstr1 and check wether str2 is substring of sstr1 
