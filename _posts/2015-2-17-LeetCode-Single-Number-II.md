---
layout: post
time: 2014-2-17
title: LeetCode --- Single Number II
category: LeetCode解题报告
keywords: LeetCode,笔试面试,算法
tags: LeetCode,异或,位运算
description: Single Number进阶，数组中只有一个数只出现一次，其他数都出现三次，找出单独的这个数。用位运算加速算法。
---

###Single Number II###

Given an array of integers, every element appears `three times` except for one. Find that single one. 

**Note:**

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

###思路：###

数组中只有一个数只出现一次，其他数都出现三次，题目要求用线性时间复杂度算法找出这个数。这道题比[Single Number][1]要难一些。

[1]: http://pinkings.github.io/leetcode%E8%A7%A3%E9%A2%98%E6%8A%A5%E5%91%8A/2014/02/15/LeetCode-Single-Number.html

考虑用统计的思路来解这道题，如果一个数出现了三次就消掉，int的32个位都是由0/1组成的，所以将所有数的对应位加起来，再将各位的统计和对3取模，最终剩下的结果就是要找出的数。如图示意(以4位为例)

![img](/assets/image/posts/Leetcode/single-number-ii.png =315x235)

###考查知识点：位运算###

###难度 : ★ ★ ★ ☆ ☆  ###

###初级解法###

用一个数组`int count[32]`,数组中`count[i]`代表第i位的当前统计值。对于每一个`A[i]`，通过移位运算获得`A[i]`各个位的值，加到count中去，最后将`count[]`中的各个数对3取模，再类似二进制转十进制的方法得到结果。如图：

![img](/assets/image/posts/Leetcode/single-number-ii-2.png =380x260)

+ 优点：思路简单，容易想到
+ 缺点：空间代价大，移位求数的每一位时间代价大。

###中级解法###

针对初级解法需要`int[32]`的空间大代价进行优化，考虑每一位的统计值，如果累加到3就归为0，则只会有0/1/2三种情况，所以将大小为32的int数组改为只用两个int即可，`int one`的第i位为0/1代表第i位当前累加有0/1个`1`，`int two`的第i位为1代表第i位当前累加有2个`1`，算法示意如图：

![img](/assets/image/posts/Leetcode/single-number-ii-3.png =315x235)

这种解法中要注意的三个小地方是： 
 
+ 当`one`的第i位溢出需要向`two`进位，`one`的第i位归0
+ 当`two`的第i位为1，`one`的第i位也累加到1时（说明出现了3次），需要同时归0
+ `one`和`two`的对应位不会同时为1

###高级解法###

再对中级解法进行优化，不用通过移位运算解析出A[i]的每一位，直接将整个数做位运算，加速算法。

假设当前已经统计了A[]中的一些数，`one`、`two`都有相应值。

对于下一个要统计的数`A[i]`，若`one`中第k位为1，`A[i]`的第k位也为1，说明出现到两次要向`two`进位，此时`two`的第k位需要由0(当`one`的第k位为1时，`two`的第k位只能是0)变为1，其他情况下`two`的第k位保持原值。所以用`one&A[i]`取出`one`和`A[i]`中同为1的位，再与`two`进行或运算，得到更新后的`two`。

{% highlight C++ %}
two |= (one & A[i]);
{% endhighlight %}

对于`one`的各个位的更新，遵循：

![img](/assets/image/posts/Leetcode/single-number-ii-4.png =280x100)

遵循异或的运算规则，可以直接用异或位运算。

{% highlight C++ %}
one ^= A[i];
{% endhighlight %}

更新完`one`和`two`之后，会有`one`和`two`的第k位同时为1的情况（即对应位出现3次），此时需要将同为1的对应位都清为0.

{% highlight C++ %}
three = one & two; //取出同为1的位
one -= three; //one中对应位清0
two -= three; //two中对用位清0
{% endhighlight %}

</br>
###解题代码：(第三种解法)###

{% highlight C++ %}
class Solution {
public:
    int singleNumber(int A[], int n) {
        int one = 0, two = 0, three = 0;
        for(int i=0; i<n; ++i) {
            two |= (one & A[i]);
            one ^= A[i];
            three = one & two;
            two -= three;
            one -= three;
        }
        return one;
    }
};
{% endhighlight %}

</br>

![img](/assets/image/daodao/5.jpg)

