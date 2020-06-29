# Cpp Basic

## const

### 1. const含义

常类型是指使用类型修饰符**const**说明的类型，常类型的变量或对象的值是不能被更新的。

### 2. const作用

- 可定义常量

```cpp
const int a = 100;
```

- 类型检查

const常量与#define宏定义常量的区别：**const常量具有类型，编译器可以进行安全检查；#define宏定义没有数据类型，只是简单的字符串替换，不能进行安全检查。**

`const` 定义的变量只有类型为整数或枚举，且以常量表达式初始化时才能作为常量表达式。其他情况下它只是一个 `const` 限定的变量，不要将与常量混淆。

- 防止修改，起保护作用，增加程序健壮性
- 可以节省空间，避免不必要的内存分配

const定义常量从汇编的角度来看，只是给出了对应的内存地址，而不是像#define一样给出的是立即数，所以，const定义的常量在程序运行过程中只有一份拷贝，而#define定义的常量在内存中有若干个拷贝。

### 3. const对象默认为文件局部变量

非const变量默认为extern。要使const变量能够在其他文件中访问，必须在文件中显式地指定它为extern。

- 未被const修饰的变量在不同文件的访问

```cpp
// file1.cpp
int ext
// file2.cpp
#include<iostream>

extern int ext;
int main(){
    std::cout<<(ext+10)<<std::endl;
}
```

- const常量在不同文件的访问

```cpp
//extern_file1.cpp
extern const int ext=12;
//extern_file2.cpp
#include<iostream>

extern const int ext;
int main(){
    std::cout<<ext<<std::endl;
}
```

可以发现未被const修饰的变量不需要extern显式声明！而const常量需要显式声明extern，并且需要做初始化！因为常量在定义后就不能被修改，所以定义时必须初始化。

### 4. 定义常量

```cpp
const int b = 10;
b = 0; // error: assignment of read-only variable ‘b’
const string s = "helloworld";
const int i,j=0 // error: uninitialized const ‘i’
```

上述有两个错误，第一：b为常量，不可更改！第二：i为常量，必须进行初始化！(因为常量在定义后就不能被修改，所以定义时必须初始化。)

### 5. 指针与const

与指针相关的const有四种：

```cpp
const char * a; //指向const对象的指针或者说指向常量的指针。
char const * a; //同上
char * const a; //指向类型对象的const指针。或者说常指针、const指针。
const char * const a; //指向const对象的const指针。
```

小结：如果*const*位于`*`的左侧，则const就是用来修饰指针所指向的变量，即指针指向为常量；如果const位于`*`的右侧，*const*就是修饰指针本身，即指针本身是常量。

具体使用如下：

#### 指向常量的指针

```cpp
const int *ptr;
*ptr = 10; //error
```

ptr是一个指向int类型const对象的指针，const定义的是int类型，也就是ptr所指向的对象类型，而不是ptr本身，所以ptr可以不用赋初始值。但是不能通过ptr去修改所指对象的值。

除此之外，也不能使用void`*`指针保存const对象的地址，必须使用const void`*`类型的指针保存const对象的地址。

```cpp
const int p = 10;
const void * vp = &p;
void *vp = &p; //error
```

另外一个重点是：**允许把非const对象的地址赋给指向const对象的指针**。

将非const对象的地址赋给const对象的指针:

```cpp
const int *ptr;
int val = 3;
ptr = &val; //ok
```

我们不能通过ptr指针来修改val的值，即使它指向的是非const对象!

我们不能使用指向const对象的指针修改基础对象，然而如果该指针指向了非const对象，可用其他方式修改其所指的对象。可以修改const指针所指向的值的，但是不能通过const对象指针来进行而已！如下修改：

```cpp
int *ptr1 = &val;
*ptr1=4;
cout<<*ptr<<endl;
```

**小结**：对于指向常量的指针，不能通过指针来修改对象的值。也不能使用void`*`指针保存const对象的地址，必须使用const void`*`类型的指针保存const对象的地址。
允许把非const对象的地址赋值给const对象的指针，如果要修改指针所指向的对象值，必须通过其他方式修改，不能直接通过当前指针直接修改。

#### 常指针

const指针必须进行初始化，且const指针的值不能修改。

```cpp
#include<iostream>
using namespace std;
int main(){

    int num=0;
    int * const ptr=&num; //const指针必须初始化！且const指针的值不能修改
    int * t = &num;
    *t = 1;
    cout<<*ptr<<endl;
}
```

上述修改ptr指针所指向的值，可以通过非const指针来修改。

最后，当把一个const常量的地址赋值给ptr的时候，由于ptr指向的是一个变量，而不是const常量，所以会报错，出现：const int`*` -> int `*`错误！

```cpp
#include<iostream>
using namespace std;
int main(){
    const int num=0;
    int * const ptr=&num; //error! const int* -> int*
    cout<<*ptr<<endl;
}
```

上述若改为const int * ptr或者改为const int * const ptr，都可以正常！

#### 指向常量的常指针

```cpp
const int p = 3;
const int * const ptr = &p; 
```

ptr是一个const指针，然后指向了一个int 类型的const对象。

### 6. 函数中使用const

> const修饰函数返回值

这个跟const修饰普通变量以及指针的含义基本相同：

- const int

```cpp
const int func1();
```

这个本身无意义，因为参数返回本身就是赋值给其他的变量！

- const int*

```cpp
const int* func2();
```

指针指向的内容不变。

- int *const

```cpp
int *const func2();
```

指针本身不可变。

> const修饰函数参数

- 传递过来的参数及指针本身在函数内不可变，无意义！

```cpp
void func(const int var); // 传递过来的参数不可变
void func(int *const var); // 指针本身不可变
```

表明参数在函数体内不能被修改，但此处没有任何意义，var本身就是形参，在函数内不会改变。包括传入的形参是指针也是一样。

输入参数采用“值传递”，由于函数将自动产生临时变量用于复制该参数，该输入参数本来就无需保护，所以不要加const 修饰。

- 参数指针所指内容为常量不可变

```cpp
void StringCopy(char *dst, const char *src);
```

其中src 是输入参数，dst 是输出参数。给src加上const修饰后，如果函数体内的语句试图改动src的内容，编译器将指出错误。这就是加了const的作用之一。

- 参数为引用，为了增加效率同时防止修改

```cpp
void func(const A &a)
```

对于非内部数据类型的参数而言，像void func(A a) 这样声明的函数注定效率比较低。因为函数体内将产生A 类型的临时对象用于复制参数a，而临时对象的构造、复制、析构过程都将消耗时间。

为了提高效率，可以将函数声明改为void func(A &a)，因为“引用传递”仅借用一下参数的别名而已，不需要产生临时对象。但是函数void func(A &a) 存在一个缺点：“引用传递”有可能改变参数a，这是我们不期望的。解决这个问题很容易，加const修饰即可，因此函数最终成为void func(const A &a)。

以此类推，是否应将void func(int x) 改写为void func(const int &x)，以便提高效率？完全没有必要，因为内部数据类型的参数不存在构造、析构的过程，而复制也非常快，“值传递”和“引用传递”的效率几乎相当。

**小结**：

对于非内部数据类型的输入参数，应该将“值传递”的方式改为“const 引用传递”，目的是提高效率。例如将void func(A a) 改为void func(const A &a)。

对于内部数据类型的输入参数，不要将“值传递”的方式改为“const 引用传递”。否则既达不到提高效率的目的，又降低了函数的可理解性。例如void func(int x) 不应该改为void func(const int &x)。

### 7. 类中使用const

在一个类中，任何不会修改数据成员的函数都应该声明为const类型。如果在编写const成员函数时，不慎修改数据成员，或者调用了其它非const成员函数，编译器将指出错误，这无疑会提高程序的健壮性。使用const关键字进行说明的成员函数，称为常成员函数。只有常成员函数才有资格操作常量或常对象，没有使用const关键字进行说明的成员函数不能用来操作常对象。

- 对于类中的const成员变量必须通过初始化列表进行初始化
- const对象只能访问const成员函数,而非const对象可以访问任意的成员函数,包括const成员函数
- 在C++中，static静态成员变量不能在类的内部初始化。在类的内部只是声明，定义必须在类定义体的外部，通常在类的实现文件中初始化

## static

当与不同类型一起使用时，static关键字具有不同的含义。我们可以使用static关键字：

**静态变量：** 函数中的变量，类中的变量

**静态类的成员：** 类对象和类中的函数

### 静态变量

- 函数中的静态变量

当变量声明为static时，空间**将在程序的生命周期内分配**。即使多次调用该函数，静态变量的空间也**只分配一次**，前一次调用中的变量值通过下一次函数调用传递。这对于在C / C ++或需要存储先前函数状态的任何其他应用程序非常有用。

```cpp
#include <iostream> 
#include <string> 
using namespace std; 

void demo() 
{ 
    // static variable 
    static int count = 0; 
    cout << count << " "; 

    // value is updated and 
    // will be carried to next 
    // function calls 
    count++; 
} 

int main() 
{ 
    for (int i=0; i<5; i++)  
        demo(); 
    return 0; 
} 
```

输出：

```cpp
0 1 2 3 4
```

由于变量count被声明为static，因此它的值通过函数调用来传递，每次调用函数时，都不会对变量计数进行初始化。

- 类中的静态变量

由于声明为static的变量只被初始化一次，因为它们在单独的静态存储中分配了空间，因此类中的静态变量**由对象共享。**对于不同的对象，不能有相同静态变量的多个副本。也是因为这个原因，静态变量不能使用构造函数初始化。

### 静态成员

- 类对象为静态

就像变量一样，对象也在声明为static时具有范围，直到程序的生命周期。

考虑以下程序，其中对象是非静态的。

```cpp
#include<iostream> 
using namespace std; 

class Apple 
{ 
    int i; 
    public: 
        Apple() 
        { 
            i = 0; 
            cout << "Inside Constructor\n"; 
        } 
        ~Apple() 
        { 
            cout << "Inside Destructor\n"; 
        } 
}; 

int main() 
{ 
    int x = 0; 
    if (x==0) 
    { 
        Apple obj; 
    } 
    cout << "End of main\n"; 
} 
```

输出：

```cpp
Inside Constructor
Inside Destructor
End of main
```

在上面的程序中，对象在if块内声明为非静态。因此，变量的范围仅在if块内。因此，当创建对象时，将调用构造函数，并且在if块的控制权越过析构函数的同时调用，因为对象的范围仅在声明它的if块内。 如果我们将对象声明为静态，现在让我们看看输出的变化。

```cpp
#include<iostream> 
using namespace std; 

class Apple 
{ 
    int i; 
    public: 
        Apple() 
        { 
            i = 0; 
            cout << "Inside Constructor\n"; 
        } 
        ~Apple() 
        { 
            cout << "Inside Destructor\n"; 
        } 
}; 

int main() 
{ 
    int x = 0; 
    if (x==0) 
    { 
        static Apple obj; 
    } 
    cout << "End of main\n"; 
} 
```

输出：

```cpp
Inside Constructor
End of main
Inside Destructor
```

可以清楚地看到输出的变化。现在，在main结束后调用析构函数。这是因为静态对象的范围是贯穿程序的生命周期。

- 类中的静态函数

就像类中的静态数据成员或静态变量一样，静态成员函数也不依赖于类的对象。我们被允许使用对象和'.'来调用静态成员函数。但建议使用类名和范围解析运算符调用静态成员。

允许静态成员函数仅访问静态数据成员或其他静态成员函数，它们无法访问类的非静态数据成员或成员函数。

```cpp
#include<iostream> 
using namespace std; 

class Apple 
{ 
    public: 
        // static member function 
        static void printMsg() 
        {
            cout<<"Welcome to Apple!"; 
        }
}; 

// main function 
int main() 
{ 
    // invoking a static member function 
    Apple::printMsg(); 
} 
```

输出：

```cpp
Welcome to Apple!
```

## this

**this指针的用处**：

1. 一个对象的this指针并不是对象本身的一部分，不会影响sizeof(对象)的结果
2. this作用域是在类内部，当在类的非静态成员函数中访问类的非静态成员的时候，编译器会自动将对象本身的地址作为一个隐含参数传递给函数。也就是说，即使你没有写上this指针，编译器在编译的时候也是加上this的，它作为非静态成员函数的隐含形参，对各成员的访问均通过this进行。

**this指针的使用**：

1. 在类的非静态成员函数中返回类对象本身的时候，直接使用 return *this
2. 当参数与成员变量名相同时，如this->n = n （不能写成n = n)

