---
aliases: 
tags:
  - STL
  - cpp
title: vector数组使用优化
date: 2024-09-19 23:36
updated: 2024-09-20 00:57
---
关于 vector 数组的基础知识可以看[[动态数组vector]]，下面将从 vector 的运行原理和代码编写两个方面讲解如何优化 vector

## 复制原理
优化的第一步便是了解优化对象
创建一个 vector，然后使用 push_back 向数组中添加元素，如果 vector 的大小无法容纳新添加的元素，则 vector 需要分配足够容纳所有元素（包括新元素）的新内存空间，然后将旧 vector 内存空间的内容复制到新空间中，并删除旧 vector 的内存空间
> [!ABSTRACT] Summary
> Vector 容量用完，则调整大小，重新分配，复制元素。这正是代码效率降低的原因之一

对于 [[动态数组vector#存储对象 or 存储指针|Vector对象数组]] 而言，复制元素会导致代码效率降低，因此需要了解复制的时候发生了什么？
 ```cpp
 #include<iostream>
 #include<vector>
 
 struct Vertex {
 	float x, y, z;
 
 	Vertex(float x, float y, float z)
 		: x(x), y(y), z(z)
 	{
 
 	}
 	Vertex(const Vertex& vertex)
 		: x(vertex.x), y(vertex.y), z(vertex.z)
 	{
 		std::cout << "Copied!" << std::endl;
 	}
 };
 
 int main() {
 	//size=0
 	std::vector<Vertex> vertices;
 	//size=1
 	vertices.push_back({1, 2, 3});
 	//size=2
 	vertices.push_back({4, 5, 6});
 }
 ```
上述代码运行了三次[[拷贝与拷贝构造函数|拷贝构造函数]]，并且Vector 的 `size` 在不断根据内部元素数量产生变化
第一次拷贝：将 `{1，2，3}` Vertex 拷贝到 vertices[0]中
空间不足，申请新的 vertices 空间
第二次拷贝：将旧 vertices 中的 `{1，2，3}` Vertex 拷入新 vertices 中
第三次拷贝：将 `{4，5，6}` Vertex 拷贝到 vertices[1]中
## 复制优化
### Vector 元素构造优化
在 vector 分配的内存中构造 vertex，可以避免 vertex 的复制行为（即第一次拷贝和第三次拷贝）
```cpp
vertices.emplace_back(1, 2, 3);
vertices.emplace_back(4, 5, 6);
```
使用 `emplace_back` 代替 push_back，传递对象构造函数的参数列表，让 vertices 在对应的内存空间初始化对象元素
### Vector 分配空间优化
在 vector 声明时为后续操作提前开辟好空间，可以避免重新分配内存时的复制行为（即第二次拷贝）
```cpp
//内存会自动生成三个vertex默认对象（无参构造函数调用）
std::vector<Vertex> vertices(3);
//仅开辟内存空间，无对象元素
vertices.reserve(3);
```
