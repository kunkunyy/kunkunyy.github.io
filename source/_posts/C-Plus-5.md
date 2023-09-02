---
title: C++学习笔记（五）引用
date: 2022-03-28 21:22:04
tags:
- C++学习
categories:
- [C++学习笔记]
- [C++面向对象]
---

# 引用的基本使用

* 作用：给变量起别名。
* 语法：```数据类型 &别名 = 原名```。

```c++
int main(){
    int a = 10;
    int &b = a;
}
```

# 注意事项

* 引用必须初始化。
    * ```int &b;//错误的```
* 引用初始化后不可被修改。

# 引用做函数参数

* 作用：函数传参时，可以利用引用的技术让形参修饰实参。
* 优点：可以简化指针修改实参。

```c++
void swap(int &a,int &b){
    int temp = a;
    a = b;
    b = temp;
}
int main(){
    int a = 10;
    int b = 20;
    swap(a,b);
    return 0;
}
```

# 引用作函数返回值

* 不要返回局部变量的引用。

```C++
int& test(){
    int a = 10;
    return a;
}
int main(){
    int &ref = test();
    cout<< ref<< endl;//10 编译器有保存
    cout<< ref<< endl;//任意值
    return 0;
}
```

* 函数的调用可以作为左值。

```C++
int& test(){
    static int a = 10;
    return a;
}
int main(){
    int &ref = test();
    cout<< ref<< endl;//10
    cout<< ref<< endl;//10

    test() = 1000;//返回值为引用

    cout<< ref<< endl;//1000
    cout<< ref<< endl;//1000
    return 0;
}
```

# 引用的本质

* 引用的本质在C++内部实现是一个指针常量。

```c++
int a = 10;
int &ref = a;//等价于int* const ref = &a;
ref = 20;//等价于 *ref = 20;
```

# 常量引用

* 主要用于修饰形参，防止误操作。
    * 在函数形参列表中，可以加const修饰形参，防止形参改变实参。

* 引用必须指向一块合法的内存空间。

```c++
int main(){
    int a = 10;
    const int &ref = 10;
    // int temp = 10; const int &ref = temp
    return 0;
}
```

```c++
void print(const int &val){
    val = 100;// 报错
    cout << val << endl;//10
}
int main(){
    int a = 10;
    print(a)
    return 0;
}
```