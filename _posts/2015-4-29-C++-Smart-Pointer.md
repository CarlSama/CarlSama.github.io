---
layout : post
time : 2015-4-29
title : Smart Pointer in C++
category : C++ 
keywrods : C++, Smart Pointer
tags : C++
description : summary about smart pointer in C++
---

## 背景

众所周知，指针的理解和使用是Ｃ语言的一大难点，特别是臭名昭著的"memory leak"问题．幸运的是，Ｃ++为程序员们引入了smart pointer,下面让我们来具体看一下．

## auto_ptr

### 构造auto_ptr

如果要在程序中使用auto_ptr，应该在程序中包含<memory>头文件，

{% highlight C++ %}
	auto_ptr<int> sp(new int(1));
{% endhighlight %}

我们在使用auto_ptr的时候有个地方需要注意，(1)不同的auto_ptr之间不可以共享使用同一指针!(想想为什么？)
{% highlight C++ %}
	int* p = new int(1);
    auto_ptr<int> sp1(p);
    auto_ptr<int> sp2(p);
{% endhighlight %}
auot_ptr中未使用引用计数，它要求获得对指针的独占使用权．

上面的程序会报出＂double free or corruption＂的错误．

(2)因为auto_ptr使用delete，而不是delete[]来释放指针，所以不可以使用auto_ptr来管理数组指针对象．
{% highlight C++ %}
	int* p = new int[10];
    auto_ptr<int> sp1(p);
{% endhighlight %}
只有首元素被释放．

(3)auto_ptr的构造函数为explicit类型，所以一个普通的指针不会隐式的转换为auot_ptr.

### 拷贝构造auto_ptr

上面提到auto_ptr的对指针的独占性．在拷贝构造函数中，会进行指针的＂占有权转移动作＂．因此，与常见的拷贝构造函数，赋值构造函数不同，auto_ptr的拷贝构造函数和复制构造函数的参数要为reference type而非const reference type.

在赋值或拷贝之后，原auto_ptr已经丧失了对指针的独占权，再用它来操作是invalid的,系统会提示"segment fault"

{% highlight C++ %}
auto_ptr<int> sp1(new int(1));
auto_ptr<int> sp2 = sp1;
cout<<*sp1<<endl;
{% endhighlight %}

因为这个原因，在程序中可能出现一种微妙的错误．当我们使用auto_ptr的形参来进行参数传递时，对指针的独占权会被转移到形参身上，此时在函数调用结束后，继续使用auto_ptr自然就产生了"segment fault".

{% highlight C++ %}
void test(auto_ptr<int> x);

auto_ptr<int> sp1(new int(1));
test(sp1);
cout<<*sp1<<endl;
{% endhighlight %}

对于这个问题，可能你会想到我们使用pointer或者reference类型来做参数传递不就行了？可是，我们无法确保函数中不会对auto_ptr进行独占权的转移操作，所以将参数类型声明为const reference是一个不错的选择． :)

在auto_ptr的拷贝构造函数和赋值构造函数中，都定义了函数模板

{% highlight C++ %}
template<class Y>
    auto_ptr(auto_ptr<Y>&rhs)throw():ap(rhs.release()){
	}
{% endhighlight %}

这可以辅助实现auto_ptr中的类层次间的隐式类型转换，
{% highlight C++ %}
class Base {};
class Derived : public Base {}

auto_ptr<Base>
template<class Y>
    auto_ptr(auto_ptr<Y>&rhs)throw():ap(rhs.release()){
	}
{% endhighlight %}

还有一点需要注意，因为Ｃ++中对STL的使用较为频繁，可是我们不难看出auto_ptr并不适宜在STL中的使用．因为auto_ptr的赋值操作会引起独占权的转移．

在auto_ptr中有三个函数：

get -> get pointer

release -> Set the internal pointer to be null without destructing the object 

reset -> deallocate the object and set new value

## To be continue ...

## Reference

[C++ Reference](http://www.cplusplus.com/reference/memory/auto_ptr/?kw=auto_ptr)
