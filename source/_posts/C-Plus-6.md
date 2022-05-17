---
title: C++学习笔记（六）函数提高
date: 2022-03-29 14:27:53
tags:
- C++学习
categories:
- [C++学习笔记]
- [C++面向对象]
---

# 函数默认参数

* 函数的形参可以有默认值。
* 语法：```返回值类型 函数名(参数 = 默认值){}```

```c++
int sum(int a,int b = 20,int c = 30){
    return a + b + c;
}
int main(){
    cout<<sum(10)<<endl;// 60
    cout<<sum(10,30,40)<<endl;// 80
    return 0;
}
```

* 注意事项：
    * 如果某个位置已经有了默认参数，那么从这个位置起，从左往右都必须有默认值。
    * 如果函数声明有默认参数，函数实现就不能有默认参数。

# 函数占位参数

* C++函数的形参列表里可以有占位参数，用来占位，调用函数时必须填补该位置。
* 语法：```返回值类型 函数名(数据类型){}```
* 占位参数可以有默认参数。

```c++
void func(int a,int = 10){
    return 'Hello World!';
}

int main(){
    func(100);
    return 0;
}
```

# 函数重载

## 概念

* 作用：函数名可以相同，提高复用性。
* 满足条件：
    * 作用域相同；
    * 函数名称相同；
    * 函数参数类型不同或者个数不同或者顺序不同。
* 函数的返回值不可以作为函数重载的条件。

```c++
void func(double a){
    cout << "int a"<<endl;
}
// 参数个数不同
void func(double a,int b){
    cout << "double a,int b"<<endl;
}
// 类型不同
void func(double a,double b){
    cout << "double a,double b"<<endl;
}
// 顺序不同
void func(int b,double a){
    cout << "int b,double a"<<endl;
}
int main(){
    func(1.5);
    func(10.5,15);
    func(10.5,15.5);
    func(15,10.5);
    return 0;
}
```

## 注意事项

* 引用作为重载条件。

```c++
void func(int &a){
    cout<< "int &a" <<endl;
}
void func(const int &a){
    cout<< "const int &a" <<endl;
}
int main(){
    int a = 10;
    func(a);// int &a
    func(10);// const int &a
    return 0;
}
```

* 函数重载碰到函数默认参数。

```c++
void func(int a){
    cout<< "int a" <<endl;
}
void func(int a,int b = 100){
    cout<< "int a,int b" <<endl;
}
int main(){
    int a = 10;
    func(10);// 报错，二义性
    func(10,20);// int a,int b
    return 0;
}
```