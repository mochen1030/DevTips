[TOC]

### 1. 字符指针、字符数组、字符指针数组

#### 1.1 字符指针 char\*、const char\*

```c++
const char* cp = "hello";

std::cout << cp << std::endl;
std::cout << cp + 1 << std::endl;
std::cout << cp[0] << std::endl;
std::cout << cp[0] + 1<< std::endl;
```

输出：

```
hello
ello
h
105
```

字符指针 (const) char\* 指向某一块连续内存中某一字符的内存地址，打印会输出从所指向该字符的内存地址开始，直至连续内存结束地址的字符值。

通常字符指针指向的都是该连续内存中首字符的地址，可以通过指针的算术操作改变所指向字符。

- **为什么字符串能赋值给指针，没有类型错误么？**

  我们把它看作是字符串，可是编译器却把它看作是地址，字符串中第一个字符的地址。虽然表象是字符串传递给指针，但是实际上是地址传递给指针，

  字符串常量存储在内存中的静态区，==字符串其实就是字符数组==，所以对于编译器来说，就是在静态区开辟内存存储该字符数组，然后把该字符数组首字符的地址传递给 const char* 类型的指针变量。对 const char* 指针通过 * 解引用即可验证。

- **为什么打印输出的是字符串的值，而不是地址？**

  这和 << 运算符的重载有关。 C++ Reference 可以看到 std::cout 的 << 运算符涉及指针时有以下几个重载：

  ```
  ostream& operator<< (void* val);
  ostream& operator<< (ostream& os, const char* s);
  ostream& operator<< (ostream& os, const signed char* s);
  ostream& operator<< (ostream& os, const unsigned char* s);
  ```

  后三种重载是用来输出 C 风格的字符串的，

- **对 char\* 类型使用下标操作，由于下标运算符的重载，会输出相应位置的单个字符。**

- **const char* 和 char* 的区别：**

  - 可以直接用 char* 初始化 const char*，反之不可以
  - const char* 可以用字符串常量进行初始化，char* 不可以
  - const char* 可以指向别的地址，但是不能通过该指针修改指针指向的内容，char* 可以



#### 1.2 字符数组 char []

```c++
char array[6] = {"hello"};

std::cout << array << std::endl;
std::cout << array + 1 << std::endl;
std::cout << array[0] << std::endl;
std::cout << array[0] + 1 << std::endl;
```

输出结果：

```
hello
ello
h
105
```

字符数组也是使用连续内存存放多个字符串，字符数组的名字，可以作为一个 char\* 类型的字符指针使用，所指向的就是数组首个字符的地址。

字符数组确定长度时会一直到 \0 结束，所以数组长度是 6，前 5 个字符分别是 hello，第 6 个是 \0。

（C/C++ 字符串以 \0 结尾，其他语言不一定是，比如 Python 不是）



#### 1.3 字符指针数组 char\* []

```c++
char array[6] = {"hello"};
char array_2[6] = {"world"};
char* cp = array;
char* cp_2 = array_2;
char* ap[2] = {cp, cp_2};

std::cout << ap << std::endl;
std::cout << ap + 1 << std::endl;
std::cout << ap[0] << std::endl;
std::cout << ap[0] + 1<< std::endl;
std::cout << ap[1] << std::endl;
std::cout << *ap << std::endl;
```

输出：

```
006FFC88
006FFC8C
hello
ello
world
hello
```

char\* [] 本质是一个数组，并不是一个指针，数组的元素是 char\* 类型。

ap 输出的是数组的首地址，首地址存放的值是 char* 元素的地址，也就是指针的指针。对 ap 解引用，打印输出的就是 char\* 数据，输出的是字符指针数组存放的第一个字符串。

ap[0] 则输出的是第一个元素，也就是 char\*，所以输出是 “hello” 字符串，同理 ap[1] 输出是 “world” 字符串



**小结：**

1. char\* 表示单个字符串，char\* 表示存放多个字符串的字符串数组
2. char\* 和 char [] 基本是一个东西，只是表达形式有差异罢了，都可以表示一个单个字符串。



**ps：**

Bjarne 在他的 The C++ Programming Language 里面给出过一个助记的方法：**把一个声明从右向左读**

```
char * const cp;
cp is a const pointer to char 

const char * p; 
p is a pointer to const char; 
```

C++ 标准规定，const 关键字放在类型或变量名之前等价的

```
const int n=5;    	// same as below
int const m=10;

const int *p;    	// same as below
int const *q;
```

==int* 不是一个类型，虽然有时候把它理解为一个类型容易接收，指针存放的值是地址，所以严格来说指针是int类型==











