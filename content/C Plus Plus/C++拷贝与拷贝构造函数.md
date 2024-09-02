拷贝是指将一个对象或者数据从一个地方复制到另一个地方，同时存在两个副本
拷贝一方面可以让代码按照预期运行，另一方不必要的拷贝会影响代码的运行效率。理解 C++中拷贝的工作原理对于编写高效的 C++代码是很有必要的
> [!TIP] TIP💡 
>  在读取或修改一个已经存在的对象时，应尽可能的避免拷贝，因为拷贝需要额外的时间开销

```cpp
struct Vector
{
	float x,y;
}

int main()
{
	int a = 2;
	int b = a;	//b与a是独立的两个内存空间
	b = 3;      //a的值并不会受到影响

	Vector a = {2,3};
	Vector b = a;		//b与a仍然是独立的两个内存空间
	b.x = 5;			//a的值不受影响

	Vector* a = new Vector();
	Vector* b = a;				//b和a的"值"相同，即地址相同
	//case1
	b++;						//a的“值”不变，即指针指向地址不变
	//case2
	b->x = 2;					//因为a和b指向同一个地址，所以a和b同时变化
	
}
```
除了引用以外，每次使用 `=` 都是在进行一次拷贝，指针会复制内存地址值，变量会复制内存中的值

下面通过实现一个字符串类
```cpp
class String
{
private:
	char* m_Buffer;
	unsigned int m_Size;
public:
	String(const char* string)
	{
		m_Size = strlen(string);
		m_Buffer = new char[m_Size+1];	//需注意字符串后需要终止符
		memcpy(m_Buffer,string,m_Size);  
  		m_Buffer[m_Size] = 0;
	}
	~String()
	{
		delete[] m_Buffer;
	}
}
```
其中关于字符数组的赋值部分存在三种方式
```cpp
//下列两种方法仅在输入的字符数组存在终止符时才可正常运行 
//strcpy将源字符数组逐一复制到目标数组，直到源字符数字的终止符停止  
strcpy(m_Buffer,string);  
//使用memcpy将源字符数组的最后一位终止符复制过来
memcpy(m_Buffer,string,m_Size+1);

//人为地增加终止符，保证字符数组是否有终止符，都不影响字符串的创建
memcpy(m_Buffer,string,m_Size);  
m_Buffer[m_Size] = 0;
```
