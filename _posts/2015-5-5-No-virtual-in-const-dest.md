---
layout : post
time : 2015-5-5
title : 探析C++中构造函数和析构函数中不能使用虚函数 
category : C++ 
keywrods : C++, Vritual Method
tags : C++
description : 探析C++中构造函数和析构函数中不能使用虚函数
---

## 背景

对于使用过C++的程序员们可能知道，virtual是C++相对于Ｃ的一个重要改变．有了virtual的支持，用户可以更好进行面向对象的设计．我们知道实物都有两面性，如果随意的使用virtual可能对你的设计产生致命的影响，这其中就不能不提到**不能在构造函数和析构函数中使用虚函数**

## 样例

我们通过一个样例来看一下不遵守规则所带来的错误．

{% highlight C++ %}
class Base {
	public:
		Base() {	test();}
	    ~Base() {	test();}
	    virtual void test() { cout<<"Base"<<endl; }
};
class Derived : public Base {
	 public:
	    Derived() {		test();}
		~Derived() {	test(); }
		void test() {	cout<<"Derived"<<endl;}
};
int main(){
	    Derived tmp;
}
{% endhighlight %}

在样例中我们构造并析构了Derived对象，我们原本希望对virtual method test的调用都会时Derived版的，但是实际的输出为
{% highlight C++ %}
clint@clint-Laptop:~$ g++ main.cpp 
clint@clint-Laptop:~$ ./a.out 
	Base
	Derived
	Derived
	Base
{% endhighlight %}

## 分析

我们知道在构造Derived对象时，是先构造其Base subobject，然后构造其Derived subobject.在构造Base subobject时，只有Base subobject的virutal method test存在，Derived subobject的virtual method test还未存在，所以只能调用Base subobject的virtual method test.

在析构Derived对象时，Derived subobject先被析构，当析构到Base subobject时，只有Base subobject的virutal method test存在，所以依然只能调用Base subobject的virtual method test
