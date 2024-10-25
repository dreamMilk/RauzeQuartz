---
aliases: 
tags: 
title: lambda函数
date: 2024-10-20 23:09
updated: 2024-10-26 00:08
---
Lambda 是定义匿名函数的方式，使用该方法创建函数，并不需要实际创建一个函数，更像是一个快速搭建的一次性函数，实现所需的功能。与其说是一个函数，其实更像是一个变量。不用通过函数定义就可以定义一个函数的方法。
# Lambda 是干什么的
只要有使用[[函数指针]]的地方，就可以使用 lambda 函数
```cpp
#include<iostream>
#include<vector>

void ForEach(const std::vector<int>& values, void(*func)(int))
{
	for(int value:values)
		func(value);
}

int main()
{
	std::vector<int> values = {1,5,4,2,3};
	
	auto lambda = [](int value) {std::cout << "Value: " << value << std::endl;}
	ForEach(values,lambda);
}
```
Lambda 函数的 `[]` 就像普通函数的参数列表一样，可以将外部变量传入到 lambda 函数中使用，同样也存在**值传递**和**引用传递**，更多细节可以查看 [lambda 表达式 (C++11 起) - cppreference.com](https://zh.cppreference.com/w/cpp/language/lambda)
```cpp
#include<iostream>
#include<vector>
#include<functional>

void ForEach(const std::vector<int>& values, const std::function<void(int)>& func)
{
	for(int value:values)
		func(value);
}

int main()
{
	std::vector<int> values = {1,5,4,2,3};
	
	int a = 5;
	
	auto lambda = [=](int value) {std::cout << "Value: " << a << std::endl;}
	
	ForEach(values,lambda);
}
```
`[=]` 表示值传递所有变量，`[&]` 表示引用传递所有变量，`[varName]` 表示传递指定变量，`[&varName]` 表示引用传递指定变量。不能使用原始函数指针，需要使用 `function` 类定义