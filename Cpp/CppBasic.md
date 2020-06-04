# Cpp Basic

## 程序流程结构

C/C++支持最基本的三种程序运行结构：顺序结构，选择结构，循环结构

- 顺序结构：程序按顺序执行，不发生跳转
- 选择结构：依据条件是否满足，有选择地执行相应功能
- 循环结构：依据条件是否满足，循环多次执行某段代码

### 1 选择结构

#### 1.1 if语句

作用：执行满足条件的语句
if语句的三种形式

- 单行格式if语句
- 多行格式if语句
- 多条件的if语句

#### 1.2 三目运算符

作用：通过三目运算符实现简单的判断

语法：表达式1 ? 表达式2 : 表达式3

解释：

如果表达式1的值为真，执行表达式2，并返回表达式2的结果；

如果表达式2的值为假，执行表达式3，并返回表达式3的结果；

```cpp
int main() {
    int a = 10;
    int b = 20;
    int c = 0;

    c = (a > b ? a : b);

    cout << "c = " << c << endl;

    (a < b ? a : b) = 100;
    cout << "a = " << a << endl;
    cout << "b = " << b << endl;

    return 0;
}
```

#### 1.3 switch语句

作用：执行多条件分支语句

### 2 循环结构

#### 2.1 while

一定要避免死循环的出现

```cpp
int main() {
    int num = 0;

    while (num < 10)
    {
        cout << num << endl;
        num++;
    }
}
```

guess number

```cpp
#include <iostream>
using namespace std;
#include <ctime>

int main() {

    srand((unsigned int)time(NULL));
    int num = rand() % 100 + 1;
    // cout << num << endl;

    int val = 0;

    while(1)
    {
        cin >> val;

        if(val > num)
        {
            cout << "猜测过大" << endl;
        }
        else if(val < num)
        {
            cout << "猜测过小" << endl;
        }
        else
        {
            cout << "恭喜您猜对了" << endl;
            break;
        }
    }
}
```

#### 2.2 do...while

```cpp
int main() {
    do
    {
        cout << num << endl;
        num++;
    }
    while (num < 10);

    return 0;
}
```

#### 2.3 for loop

```cpp
int main() {
    for(int i = 1; i <= 100; i++)
    {
        if(i % 7 == 0 || i % 10 == 7 || i / 10 == 7)
        {
            cout << "Knock desk" << endl;
        }
        else
        {
            cout << i << endl;
        }
    }
}
```
