---
comments: true
---
# C++中的Lambda表达式

C++ 11引入了lambda表达式，允许使用内联函数，这些函数可以用于不打算重用的短代码片段，因此不需要名称。在最简单的形式中，lambda表达式可以定义如下：

```
[ capture clause ] (parameters) -> return-type  
{   
   definition of method   
} 
```

C++ 11引入了lambda表达式，允许使用内联函数，这些函数可以用于不打算重用的短代码片段，因此不需要名称。在最简单的形式中，lambda表达式可以定义如下：

```cpp
// C++ program to demonstrate lambda expression in C++
#include <bits/stdc++.h>
using namespace std;
 
// Function to print vector
void printVector(vector<int> v)
{
    // lambda expression to print vector
    for_each(v.begin(), v.end(), [](int i)
    {
        std::cout << i << " ";
    });
    cout << endl;
}
 
int main()
{
    vector<int> v {4, 1, 3, 5, 2, 3, 1, 7};
 
    printVector(v);
 
    // below snippet find first number greater than 4
    // find_if searches for an element for which
    // function(third argument) returns true
    vector<int>:: iterator p = find_if(v.begin(), v.end(), [](int i)
    {
        return i > 4;
    });
    cout << "First number greater than 4 is : " << *p << endl;
 
 
    // function to sort vector, lambda expression is for sorting in
    // non-increasing order Compiler can make out return type as
    // bool, but shown here just for explanation
    sort(v.begin(), v.end(), [](const int& a, const int& b) -> bool
    {
        return a > b;
    });
 
    printVector(v);
 
    // function to count numbers greater than or equal to 5
    int count_5 = count_if(v.begin(), v.end(), [](int a)
    {
        return (a >= 5);
    });
    cout << "The number of elements greater than or equal to 5 is : "
         << count_5 << endl;
 
    // function for removing duplicate element (after sorting all
    // duplicate comes together)
    p = unique(v.begin(), v.end(), [](int a, int b)
    {
        return a == b;
    });
 
    // resizing vector to make size equal to total different number
    v.resize(distance(v.begin(), p));
    printVector(v);
 
    // accumulate function accumulate the container on the basis of
    // function provided as third argument
    int arr[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    int f = accumulate(arr, arr + 10, 1, [](int i, int j)
    {
        return i * j;
    });
 
    cout << "Factorial of 10 is : " << f << endl;
 
    //     We can also access function by storing this into variable
    auto square = [](int i)
    {
        return i * i;
    };
 
    cout << "Square of 5 is : " << square(5) << endl;
}
```

**输出：**

```
4 1 3 5 2 3 1 7 
First number greater than 4 is : 5
7 5 4 3 3 2 1 1 
The number of elements greater than or equal to 5 is : 2
7 5 4 3 2 1 
Factorial of 10 is : 3628800
Square of 5 is : 25
```

通过访问外层作用域中的变量，lambda 表达式比普通函数更强大。我们可以通过三种方式从外层作用域捕获外部变量： 
   按引用捕获 
   按值捕获
   按引用和值混合捕获
捕获变量的语法如下：
   [&]：按引用捕获所有外部变量 
   [=]：按值捕获所有外部变量
   [a, &b]：按值捕获a，按引用捕获b
具有空捕获子句[ ]的lambda只能访问其局部变量。
以下是不同捕获方法的演示：

```cpp
// C++ program to demonstrate lambda expression in C++
#include <bits/stdc++.h>
using namespace std;
 
int main()
{
    vector<int> v1 = {3, 1, 7, 9};
    vector<int> v2 = {10, 2, 7, 16, 9};
 
    //  access v1 and v2 by reference
    auto pushinto = [&] (int m)
    {
        v1.push_back(m);
        v2.push_back(m);
    };
 
    // it pushes 20 in both v1 and v2
    pushinto(20);
 
    // access v1 by copy
    [v1]()
    {
        for (auto p = v1.begin(); p != v1.end(); p++)
        {
            cout << *p << " ";
        }
    };
 
    int N = 5;
 
    // below snippet find first number greater than N
    // [N]  denotes,   can access only N by value
    vector<int>:: iterator p = find_if(v1.begin(), v1.end(), [N](int i)
    {
        return i > N;
    });
 
    cout << "First number greater than 5 is : " << *p << endl;
 
    // function to count numbers greater than or equal to N
    // [=] denotes,   can access all variable
    int count_N = count_if(v1.begin(), v1.end(), [=](int a)
    {
        return (a >= N);
    });
 
    cout << "The number of elements greater than or equal to 5 is : "
         << count_N << endl;
}
```

**输出：**

```
First number greater than 5 is : 7
The number of elements greater than or equal to 5 is : 3
```

注意：Lambda表达式在C++ 11之后可用。