#### 1. 字符指针、字符数组、字符指针数组

##### 1.1 字符指针 char\*、const char\*

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



##### 1.2 字符数组 char []

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



##### 1.3 字符指针数组 char\* []

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



##### 小结：

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



#### 2. 字符串转换

1. **const char\***

- to QString

  ```
  const char* data = "xxx";
  QString str = QString::fromLocal8Bit(data);
  ```

- to std::string

  ```
  const char* data = "xxx";
  std::string str = data;
  ```

- to char\*

  ```
  const char* data = "xxx";
  char* str = (char*)data;
  ```

- to CString

  ```
  const char* data = "xxx";
  char* str = (char*)data;
  CString cstr = _T(str);
  ```



2. **std::string**

- to QString

  ```
  std::string str = "xxx";
  QString string = QString::fromStdString(str);
  
  //中文乱码时
  std::string str = "这是一个字符串";
  QByteArray array = QByteArray::fromStdString(str);
  QString string = QString::fromLocal8Bit(array);
  ```

- const char\*

  ```
  std::string str = "xxx";
  const char* = str.c_str();
  ```

- to CString

  ```
  std::string str="test";
  CString cstrTest;
  cstrTest= CA2W(str.c_str());
  ```



3. **QString**

- to std::string

  ```
  QString str = "xxx";
  std::string string = str.toStdString();
  ```

- to const char\*

  ```
  QString str = "xxx";
  QByteArray byte = str.toLocal8Bit();
  
  const char* data = byte.data();
  or
  const char* data = str.toStdString().c_str();
  ```



4. **CString**

- to std::string

  ```
  CString cstr = CString("xxx");
  std::string str = CStringA(cstr);
  ```



5. **LPWSTR**

- to char*

  ```
  LPWSTR path = xxx;
  int nInputStrLen = wcslen(path);
  int nOutputStrLen = WideCharToMultiByte(CP_ACP, 0, path, nInputStrLen, NULL, 0, 0, 0) + 2;
  char* data = new char[nOutputStrLen];
  if (data != nullptr)
  {
      memset(data, 0x00, nOutputStrLen);
      WideCharToMultiByte(CP_ACP, 0, path, nInputStrLen, data, nOutputStrLen, 0, 0);
  }
  ```



#### 3. void\* 指针

- **void**

  函数无返回值，返回类型为 void
  函数无参数，参数列表为 void，通常省略
  void 类型的变量无意义，一般编译器会报错。把变量强制转换成 void，可以避免参数未使用的警告

- void\*



void*
函数返回值类型是void*，可以返回任何类型的指针，但必须返回一个指针。可以返回为空指针，但是不能返回为空。
函数的参数类型是void*，可以传递任何类型的指针
void*类型的指针无类型，可以被任何类型的指针赋值。
Tips
void*指针需要强制类型转换之后才可以赋值/传参给其他类型的指针,反之不需要。
void*指针可以直接和其他类型的指针进行地址的比对。
void*指针需要强制类型转换之后才可以对其进行操作。
void*指针可以初始化为空指针nullptr。
void*指针不能进行算术操作（算术类型的指针进行算术操作，解引用后可以得到其他的数值）
函数指针
基本概念
指针函数：指针函数本质是函数，该函数的返回值类型是指针。 QPushButton* generateButton(QString qsText) { QPushButton* btn = new QpushButton(); btn->setText(qsText); return btn; }

函数指针：函数指针本质是指针，该指针指向某一函数的首地址。 #include <iostream>

int testFuntion(int a, int b)
{
    return a + b;
}

int func(int c, int d, int (*pFunc)(int, int))
{
    return (*pFunc)(c, d);
    //return pFunc(c, d);
}

int main(int argc, char *argv[])
{
    //声明一个函数指针，并用函数名初始化，该指针指向函数testFuntion的首地址
    //C++会隐式地将函数名testFuntion转换成&testFuntion，所以函数指针初始化的时候可以省略&
    int (*ptr)(int, int);
    ptr = testFuntion;

    //也可以像一般指针类型一样直接声明并初始化
    //int (*ptr)(int, int) = testFuntion;    
    
    //输出地址内容一致（可能会输出1或true，取决于是否用了boolalpha，因为隐式转换到bool）
    std::cout << "testFuntion:" << testFuntion << std::endl;
    std::cout << "ptr:" << ptr << std::endl;
    
    //通过函数指针调用函数
    std::cout << "ptr result:" << (*ptr)(10, 5) << std::endl;   //建议写法
    std::cout << "ptr result:" << ptr(10, 5) << std::endl;      //一般编译器不会报错
    
    //这样类似于函数指针分方式调用函数也是可以的，但是不建议这么写
    std::cout << "testFuntion result:" << (*testFuntion)(10, 5) << std::endl;
    std::cout << "testFuntion result:" << testFuntion(10, 5) << std::endl;
    
    //函数指针作为函数的参数传递
    std::cout << "func:" << func(99, 111, ptr) << std::endl;
    std::cout << "func:" << func(99, 111, testFuntion) << std::endl;
    
    return 0;
}
=======
#### void 指针

##### void

*   函数无返回值，返回类型为 void
*   函数无参数，参数列表为 void，通常省略
*   void 类型的变量无意义，一般编译器会报错。把变量强制转换成 void，可以避免参数未使用的警告

##### void\*

*   函数返回值类型是 void\*，可以返回任何类型的指针，但必须返回一个指针。可以返回为空指针，但是不能返回为空。
*   函数的参数类型是 void\*，可以传递任何类型的指针
*   void\* 类型的指针无类型，可以被任何类型的指针赋值。

##### Tips

1.  void\* 指针需要强制类型转换之后才可以赋值/传参给其他类型的指针，反之不需要
2.  void\* 指针可以直接和其他类型的指针进行地址的比对
3.  void\* 指针需要强制类型转换之后才可以对其进行操作
4.  void\* 指针可以初始化为空指针 nullptr
5.  void\* 指针不能进行算术操作



#### 函数指针

##### 基本概念

* 指针函数：指针函数本质是函数，该函数的返回值类型是指针。

    ```c++
    QPushButton* generateButton(const QString& qsText)
    {
        QPushButton\* btn = new QPushButton();
        btn->setText(qsText);
        return btn;
    }
    ```

* 函数指针：函数指针本质是指针，该指针指向某一函数的首地址。

        #include <iostream>
        
        int testFuntion(int a, int b)
        {
            return a + b;
        }
        
        int func(int c, int d, int (*pFunc)(int, int))
        {
            return (*pFunc)(c, d);
            // return pFunc(c, d);
        }
        
        int main(int argc, char* argv[])
        {
            // 声明一个函数指针，并用函数名初始化，该指针指向函数 testFuntion 的首地址
            // C++ 会隐式地将函数名 testFuntion 转换成 &testFuntion，所以函数指针初始化的时候可以省略 &
            int (*ptr)(int, int);
            ptr = testFuntion;
            
            // 也可以像一般指针类型一样直接声明并初始化
            // int (*ptr)(int, int) = testFuntion;    
        
            // 输出地址内容一致（可能会输出1或true，取决于是否用了boolalpha，因为隐式转换到bool）
            std::cout << "testFuntion:" << testFuntion << std::endl;
            std::cout << "ptr:" << ptr << std::endl;
        
            // 通过函数指针调用函数
            std::cout << "ptr result:" << (*ptr)(10, 5) << std::endl;   //建议写法
            std::cout << "ptr result:" << ptr(10, 5) << std::endl;      //一般编译器不会报错
        
            // 这样类似于函数指针分方式调用函数也是可以的，但是不建议这么写
            std::cout << "testFuntion result:" << (*testFuntion)(10, 5) << std::endl;
            std::cout << "testFuntion result:" << testFuntion(10, 5) << std::endl;
        
            // 函数指针作为函数的参数传递
            std::cout << "func:" << func(99, 111, ptr) << std::endl;
            std::cout << "func:" << func(99, 111, testFuntion) << std::endl;
        
            return 0;
        |

##### Tips:

1. 函数指针的返回值类型，参数列表必须和其指向的函数保持一直，参数名可以省略。

2. 只要返回值类型，参数列表一致，同一函数指针可以指向不同的函数。

3. 函数指针如果只由基本内置类型组合而成，则可以像基本类型指针一样直接声明使用，而不是像类一样必须先存在此类，才能声明此类的指针。

4. 函数指针在回调函数中使用较多

5. typedef 简化函数指针的定义

    ```
    int func(int a, double c, char* c);
    {
        ...
        return 0;
    }
    
    int main(int argc, char *argv[])
    {
        // 利用 typedef 将函数指针声明成一个类型，此类型名为FuncPtr，函数指针名名为 ptr
        typedef int (*FuncPtr)(int a, double c, char* c);
        FuncPtr ptr = func;
    
        // 并未将函数指针声明成一个类型，此函数指针名就是 ptr
        // int (*ptr)(int a, double c, char* c) = func;
    
        return 0;
    }
    ```



#### 野指针

- 指针未初始化

    指针被创建但是未初始化，因为其缺省值是随机的，所以指针不会默认被初始化为空指针，而是会随机指向其他内存块。

- 指针析构后未置空

    free 或 delete 指针之后，需要手动将指针置空（nullptr）。析构指针只是将指针指向的内存释放，指针本身依然占用内存，所以该指针的声明周期并未结束，指针指向的内存块还是那块被析构的指针，其值也不等于 nullptr，使用 **ptr == nullptr **并不会得到 true值。


- 指针指向的内容生命周期已结束

    针指向的内容生命周期结束之后，指针依然指向那块被释放的内存快，原理同上。
    （空指针只是不指向任何东西，其本身依然占用内存）



#### 字符串转换

##### 1. const char\*

* to QString

    ```c++
    const char* data = "xxx";
    QString str = QString::fromLocal8Bit(data);
    ```

* to std::string

    ```c++
    const char* data = "xxx";
    std::string str = data;
    ```

* to char\*

    ```c++
    const char* data = "xxx";
    char* str = (char*)data;
    ```

* to CString (MFC)

    ```c++
    const char* data = "xxx";
    char* str = (char*)data;
    CString cstr = _T(str);
    ```

##### 2. std::string

* to QString

    ```c++
    std::string str = "xxx";
    QString string = QString::fromStdString(str);
    
    // 中文乱码时
    std::string str = "这是一个字符串";
    QByteArray array = QByteArray::fromStdString(str);
    QString string = QString::fromLocal8Bit(array);
    ```

* to const char\*

    ```c++
    std::string str = "xxx";
    const char* = str.c_str();
    ```

* to CString (MFC)

    ```c++
    std::string str="test";
    CString cstrTest;
    cstrTest= CA2W(str.c_str());
    ```

##### 3. QString

* std::string

    ```c++
    QString str = "xxx";
    std::string string = str.toStdString();
    ```

* to const char\*

    ```c++
    QString str = "xxx";
    QByteArray byte = str.toLocal8Bit();
    const char* data = byte.data();
    ```

##### 4. CString

* to std::string

        CString cstr = CString("xxx");
        std::string str = CStringA(cstr);

##### 5. LPWSTR

* to char\*

        LPWSTR path = xxx;
        int nInputStrLen = wcslen(path);
        int nOutputStrLen = WideCharToMultiByte(CP_ACP, 0, path, nInputStrLen, NULL, 0, 0, 0) + 2;
        char* data = new char[nOutputStrLen];
        if (data != nullptr)
        {
        	memset(data, 0x00, nOutputStrLen);
        	WideCharToMultiByte(CP_ACP, 0, path, nInputStrLen, data, nOutputStrLen, 0, 0);
        }



#### 函数返回const char\* 乱码

踩坑场景： string 直接转 const char\* 作为函数返回值有可能会乱码，本人踩坑时，场景是返回字符串长度不超过15就会乱码。

```cpp
const char* myfunction()
{
    std::string str = "This is a test string";
    const char* data = str.c_str();
    return data;
}
```

解决方案：

```cpp
const char* myfunction()
{
    std::string str = "This is a test string";
    int sum = str.length();
    char* dataArray = new char[sum];
    for (int i = 0; i < sum; ++i)
    {
        dataArray[i] = str[i];
    }
    dataArray[sum] = '\0';          //添加结束符
    const char* data = dataArray();
    return data;
}
```
