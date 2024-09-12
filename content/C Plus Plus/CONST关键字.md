---
aliases: 
tags:
  - cpp
title: CONST关键字
date: 2024-09-04 20:20
updated: 2024-09-12 23:12
---

`const` 不像其他关键字一样，可以改变代码，而是像一种约束代码的机制，它对声明的东西承诺永不改变，如果你承诺（声明）过，那么请你遵守它，这会对你的代码习惯有所提升。
> [!WARNING] WARNING⚠️
>  虽然它只是承诺，存在其他的方法可以打破承诺，但是请不要这样做。
>  PS：使用指针指向常数内存，然后通过指针对内存中的值进行修改，可以绕过 const 限制

# 在变量中使用
```cpp
#include<iostream>
#include<string>

int main()
{
	const int a = 5;
	a = 2;	//不可改变变量值
	
	const int* b = new int;
	*b = 2;			//不可改变指针指向的内容
	b = (int*)&a;	//可改变指针指向的对象
	
	int* const c = new int;
	*c = 2;			//可改变指针指向的内容
	c = (int*)&a;	//不可改变指针指向的对象
} 
```
对于 `const type*` 与 `type* const` 需要仔细区分，两者对限制的对象完全不同

# 在类和方法中使用
```cpp
#include<iostream>
#include<string>

class Entity {
	int m_X;
	mutable int var;
public:
	int getX() const
	{
		var = 2;
		return m_X;
	}
};
```

在类方法后增加 `const` 关键字，表示该方法**不改变类中任何属性**。在 `const` 关键字声明的函数中需要改变的变量，需要使用 `mutable` 声明，否则不能改变

```cpp
#include<iostream>
#include<string>

class Entity {
	int m_X;
public:
	int getX() const
	{
		return m_X;
	}
};
```