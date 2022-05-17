---
title: C++学习笔记（七）类和对象——C++对象模型和this指针
date: 2022-04-11 21:38:35
tags:
- C++学习
categories:
- [C++学习笔记]
- [C++面向对象]
---

# 成员变量和成员函数分开存储

* 成员变量和成员函数市分开存储的。
* 非静态成员变量，属于类对象；静态成员变量，不属于类对象。
* 静态成员函数和非静态成员函数均不属于类对象。

# this指针

* this指针指向被调用的成员函数所属的对象
* 每个非静态成员函数内都有this指针。
* this指针不需要定义，直接使用。

* 用途：
    * 当形参和成员变量同名时，可以用this指针来区分
    * 在类的非静态成员函数中返回对象本身，可使用return *this

```c++
#include<iostream>
using namespace std;

class Person{
public:
    int age;
    Person(int age){
        this->age = age;
    }
    Person& PersonAddAge(Person &p){
        this->age += p.age;
        return *this; 
    }
}
void test01(){
    Person p(18);
    cout<< p.age <<endl;
}
void test01(){
    Person p1(10);
    Person p2(10);
    p2.PersonAddAge(p1);
    cout<< p2.age <<endl;// 20
}
int main(){
    cout<<  <<endl;
    return 0;
}
```

# 空指针访问成员函数

```c++
#include<iostrean>
using namespace std;

class Person{

public:
    void showClassName(){
        cout << "this is Person Class" << endl;
    }
    void showPersonAge(){

        // 提高代码的健壮性
        if(this == NULL){
            return;
        }
        cout << "age = " << m_Age << endl;
        // 等价于cout << "age = " << this.m_Age << endl;
    }
    int m_Age;
}

void test01(){
    // 空指针可以访问成员
    Person *p = NULL;
    p->showClassName();
    p->showPersonAge(); // ！报错 传入的指针为空
}

int main(){


    system("pause");
    return 0;
}
```

# const修饰成员函数

* 常函数
    * 成员函数后加const后我们称为这个函数为常函数。
    * 常函数内不可以修改成员属性。
    * 成员属性声明时加关键字mutable后，在常函数中依然可以修改。

* 常对象:
    * 声明对象前加const称该对象为常对象。
    * 常对象只能调用常函数

```c++
#include<iostrean>
using namespace std;

class Person{
public:
    // 常函数
    // const实际上是用于修饰this指针，
    // 使其在指向不可修改的基础之上，让其指向的指也不能修改
    // this 等价于 Person * const this;
    void showPerson() const{
        m_Age;//等价于this.m_Age
    }
    int m_Age;
    mutable int m_name;// 在常函数中可以修改
}

int main(){
    // 常对象
    const Person p;
    // 常对象只能调用常函数
    p.showPerson();
    system("pause");
    return 0;
}
```