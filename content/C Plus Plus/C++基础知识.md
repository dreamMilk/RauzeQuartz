---
created: 2024-08-30T01:37
updated: 2024-08-31T18:02
---

# 字符串

## C 语言中的字符串

C 语言中字符串就是一个字符数组，通常使用`const char*`表示，但是相较于字符数组，字符串会在最后一个元素添加终止符`\0`，在输出字符串时知道在何处结束

```cpp
#include <iostream>
const char* name = "ZhangSan";
char name2[8] = {'Z','h','a','n','g','S','a','n'};
char name3[9] = {'Z','h','a','n','g','S','a','n','\0'};
char name4[9] = {'Z','h','a','n','g','S','a','n',0};

std::cout << name << std::endl;   //ZhanSan
std::cout << name2 << std::endl;  //ZhanSan....未知字符
std::cout << name3 << std::endl;  //ZhanSan
std::cout << name4 << std::endl;  //ZhanSan
```

上述代码中`name2`会在内存中一直找到终止符后停止输出，因此在输出的末尾会出现未知字符。如果在字符数组最后增加终止符（或者对应的 ASCII 码）即可正确输出

## C++中的字符串

在 C++中使用双引号包裹的多个字符本质上是`const char*`，在字符串赋值时会通过`char*`的隐式转换成为字符串

```cpp
#include<iostream>
#include<string>

std::string name = "ZhangSan";     //“ZhangSan”其实是const char*
std::cout << name << std::endl;
```

头文件`<iostream>`中已经包含 string 的定义，可以直接使用。但是输出字符串时，未导入头文件`<string>`会出现错误，提示无法输出字符串到流中，因为`<<`操作符关于字符串操作的重载是在`<string>`中实现的。

C 语言中处理字符数组`const char*`的函数`strlen`，`strcpy`等函数均可在 C++的字符串中找到对应的函数

```cpp
std::string name = "ZhangSan" + " hello!";              //ERROR
std::string name = "ZhangSan";
name += " hello!";                                      //OK
std::string name = std::string("ZhangSan")+" hello!";   //OK
std::cout << name << std::endl;
```

不可以直接将两个字符串常量相加，因为两个字符串常量其实是`const char*`，因此两个指针相加是错误操作。正确的字符串拼接方式是拆分成两步，将字符串变量与`const char*`相加，因为`+`操作被`<string>`重载后，可以进行字符串与字符数组指针相加操作

```cpp
// 使用值传递会出现字符串复制，影响程序性能
void PrintString(std::string str){
	std::cout << string << std::endl;
}
//应使用引用传递，并且保证string不可修改
void PrintString(const std::string& str)
{
	std::cout << string << std::endl;
}
```

# 隐式转换与 explicit 关键字

```cpp
//类定义
class Entity
{
private:
    std::string m_Name;
    int m_Age;
public:
    Entity(std::string name):m_Name(name),m_Age(-1){}
    Entity(int age):m_Name("Unknown"),m_Age(age){}
};

void printEntity(const Entity& entity){}
```

## 隐式转换

类中定义只有一个参数的构造函数时，可以使用如下操作实例化对象

```cpp
Entity a = 22;            //等价Entity(22)
Entity b = "ZhangSan";    //等价Entity("ZhangSan")
```

在上述操作中存在着一次隐式转换，即`Entity a = Entity(22) = 22`，但是隐式转换仅调用一次

```cpp
printEntity(22);                 //OK
printEntity("ZhangSan");         //ERROR
printEntity(Entity("ZhangSan"))  //OK
```

`Zhangsan`属于 char 型数组，需要经历两次转换，第一次`char型数组=>string`，第二次`string=>Entity`，所以出现执行错误

## explicit 关键字

在构造函数前面增加`explicit`关键字可以禁止上述的隐式转换，避免出现未知的类型转换，只能通过显式构造函数实例化对象，使得代码更加清晰

