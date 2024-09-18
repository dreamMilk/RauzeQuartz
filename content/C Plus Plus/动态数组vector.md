---
aliases: 
tags:
  - STL
title: 动态数组vector
date: 2024-09-18 22:33
updated: 2024-09-19 00:49
---
C++标准模板库（STL）本质上是一个库，由容器和容器类型组成。
容器包含的数据类型由使用者决定，所有底层数据类型由[[模板]]处理

>[!lol] Funny
 **动态数组为什么叫 vector?**
 其实使用 arrayList 能够更好的理解，但由于[历史问题](https://en.wikipedia.org/wiki/Sequence_container_(C%2B%2B)#History)使用 vector，因此沿用至今 

```cpp
#include<iostream>  
#include<string>  
  
struct Vertex {  
    float x,y,z;  
};  
  
int main()  
{  
    //栈中分配  
    Vertex vertices[5];  
    //堆中分配  
    Vertex* vertices = new Vertex[5];  
}
```

无论是在栈中分配还是在堆中分配，传统数组定义时必须声明数组大小。
当输入超过数组大小时，只能重新调整数组大小，或者在定义时申请超级大（例如 5000000）。申请巨量的空间在大多数情况下会浪费，因此需要一种可变容量的数组。

```cpp
int main()  
{  
    //可以传入类类型  
    std::vector<Vertex> vertices;  
    //也可以传入基础类型，Java只能传入类类型  
    std::vector<int> vecInterge;  
}
```

# 存储对象 or 存储指针
使用 vector 存储对象还是指针主要根据使用场景决定

|                | 存储对象                  | 存储指针                   |
| -------------- | --------------------- | ---------------------- |
| 内存分布           | 对象在一条连续的内存空间中         | 对象的地址被连续存放，但对象在不连续的空间中 |
| 数组操作（遍历、更改和读取） | 速度更优                  | 对象分散，需要访问不同的内存块        |
| 数组调整大小         | 需要空间较大，数组内容复制到新数组比较麻烦 | 速度更优                   |

> [!ABSTRACT] Summary
> 在频繁操作数组时，可以选择存储对象
> 在频繁调整数组时，可以选择存储指针
> 

# 数组操作

```cpp
#include<iostream>  
#include<vector>  
  
struct Vertex {  
    float x, y, z;  
};  
  
std::ostream& operator<<(std::ostream& stream, const Vertex& vertex) {  
    stream << vertex.x << ", " << vertex.y << ", " << vertex.z;  
    return stream;  
}  
  
void Function(const std::vector<Vertex>& vertices) {  
  
}  
  
int main() {  
    std::vector<Vertex> vertices;  
    vertices.push_back({1, 2, 3});  
    vertices.push_back({4, 5, 6});  
    Function(vertices);  
  
    for (int i = 0; i < vertices.size(); i++) {  
        std::cout << vertices[i] << std::endl;  
    }  
  
    for (Vertex& v : vertices) {  
        std::cout << v << std::endl;  
    }  
  
    vertices.erase(vertices.begin() + 1);  
    vertices.clear();  
}
```