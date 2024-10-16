---
aliases: 
tags:
  - cpp
  - array
title: 静态数组array
date: 2024-10-16 22:18
updated: 2024-10-16 23:24
---
静态数组是指创建数组时，定义数组大小和数组类型，后续将不可改变，数组大小将是一个常数
与之相对应的是[[动态数组vector]]，动态数组长度可以变大或变小

```cpp
#include<iostream>
#include<array>

//使用模板可以不声明数组大小
template<int size>
void PrintArray(const std::array<int, size>& data){
	for(int i=0;i<data.size();i++){
		
	}
}

int main(){
	//两者等价
	std::array<int,5> data;
	int old_data[5];
	PrintArray<5>(data);
}
```

`array` 和传统数组的工作原理一样，并且都是在栈中分配内存，而 `vector` 在堆中分配内存。`array` 相较于传统数组而言，会存在越界检查功能，在编译时提示越界访问，可以通过宏关闭检测功能，在最优化的情况下，与传统数组的运行效率一致
> [!TIP] TIP 
>  `array` 类中并未使用一个变量去存储数组大小，而是利用模板功能定义 `size` 函数直接返回模板参数中的数组大小，也就是一个常量

`array` 数组完全可以替换传统数组，在调试时可以提醒越界访问，在发布时可以和传统数组一样高效运行，并且会十分优雅的返回数组长度