---
title: C++学习笔记（七）类和对象——运算符重载
date: 2022-04-11 21:58:43
tags:
- C++学习
categories:
- [C++学习笔记]
- [C++面向对象]
---

* 运算符重载概念：对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型

# 加号运算符重载

* 实现两个自定义数据类型相加的运算。

* 对于内置的数据类型的表达式不能改变。
* 不能滥用运算符重载。

```c++
#include<iostrean>
#include<string>
using namespace std;
class Person{
public:
    // 1、成员函数实现重载加号
    Person operator+(Person &p){
        Person tmp;
        tmp.m_a = this.m_a + p.m_a;
        tmp.m_b = this.m_b + p.m_b;
        return tmp;
    }
    int m_a;
    int m_b;
};

// 2、全局函数实现重载加号
Person operator+(Person &p1,Person &p2){
    Person tmp;
    tmp.m_a = p1.m_a + p2.m_a;
    tmp.m_b = p1.m_b + p2.m_b;
    return tmp;
}
// 运算符重载会发生函数重载
Person operator+(Person &p1,int num){
    Person tmp;
    tmp.m_a = p1.m_a + num;
    tmp.m_b = p1.m_b + num;
    return tmp;
}

void test1(){
    Person p1;
    p1.m_a = 10;
    p1.m_b = 10;
    Person p2;
    p2.m_a = 10;
    p2.m_b = 10;
    /*
        成员函数重载等价于Person p3 = p1.operator+(p2);
        全局函数重载等价于Person p3 = operator+(p1,p2);
    */
    Person p3 = p1 + p2;
    cout<< p3.m_a,p3.m_b << endl;//20,20
    Person p4 = p1 + 10;
}

int main(){
    test1()
    system("pause");
    return 0;
}
```

# 左移运算符重载

```c++
#include<iostrean>
#include<string>
using namespace std;
class Person{
public:
    // 成员函数不能实现左移运算符的重载
    int m_a;
    int m_b;
}

// 2、全局函数实现重载左移运算符
ostream & operator<<(ostream &cout,Person &p){
    cout<< p.m_a << p.m_b;
    return cout;
}

void test1(){
    Person p;
    p.m_a = 10;
    p.m_b = 10;
    cout<< p << endl;//10,10
}

int main(){
    test1()
    system("pause");
    return 0;
}
```

# 递增运算符重载

* 通过重载递增运算符实现自己的整型数据

```c++
#include<iostrean>
using namespace std;
class MyInteger{
public:
    MyInteger(){
        num = 0;
    }
    // 重载前置++
    // 返回引用是为了一直对同一个数据进行操作
    MyInteger& operator++(){
        num++;
        return *this;
    }
    //重载后置++
    // 使用占位参数区分
    MyInteger operator++(int){
        // 先记录当前结果
        MyInteger tmp = *this;
        // 加操作
        num++;
        // 返回
        return tmp;
    }
private:
    int num;
}

ostream& operator<<(ostream& cout, MyInteger& myint){
    cout << myinit.num;
    return cout;
}

int main(){

    MyInteger myinit;
    cout << ++(++myint) << endl;//2
    cout << myinit << endl;//2
    system("pause");
    return 0;
}
```

# 赋值运算符重载

C++编译器至少给一个类添加4个函数：
* 默认构造函数(无参，函数体为空)
* 默认析构函数(无参，函数体为空)
* 默认拷贝构造函数，对属性进行值拷贝
* 赋值运算符operator=,对属性进行值拷贝

如果类中有属性指向堆区，做赋值操作时也会出现深浅拷贝问题。

* **编译器默认提供的复制运算符为浅拷贝**。
* **当有数据存储在堆区的时候就会出现重复释放的问题，所以需要重新对复制运算符进行重载**。

```c++
#include<iostrean>
using namespace std;
class Person{
public:
    // 构造函数
    Person(int age){
        //将数据创建在堆区
        m_age = new int(age);
    }
    // 析构函数
    ~Person(){
        if(m_age !=NULL){
            delete m_age;
            m_age = NULL;
        }
    }
    // 重载赋值运算符
    Person& operator=(Person &p){
        if(m_age!=NULL){
            delete m_age;
            m_age = NULL;
        }
        // 深拷贝
        m_age = new int(*p.m_age);
        return *this;
    }
    int *m_age;
}

int main(){
    Person p1(18);
    Person p2(20);
    Person p3(30);
    p3 = p2 = p1;
    cout<< *p1.m_age << *p2.m_age << *p3.m_age <<endl;
    system("pause");
    return 0;
}
```

# 关系运算符重载

* 让两个自定义类型对象进行对比操作。

```c++
#include<iostrean>
#include<string>
using namespace std;

class Person{
public:
    Person(string name, int age){
        m_name = name;
        m_age = age;
    }
    // 重载==
    bool operator==(Person &p){
        if(this->n_name == p.m_name && this->m_age == p.m_age){
            return true;
        }
        return false;
    }
    //重载!=
    bool operator==(Person &p){
        if(this->n_name == p.m_name && this->m_age == p.m_age){
            return false;
        }
        return true;
    }
    string m_name;
    int m_age;
}

void test(){
    Person p1("Tom",18);
    Person p2("Tom",18);
    if(p1==p2){
        cout<< "p1=p2" <<endl;
    }
}

int main(){
    test();
    system("pause");
    return 0;
}
```

# 函数调用运算符重载

* 函数调用运算符()也可以重载。
* 由于重载后使用的方式非常像函数的调用，因此称为**仿函数**
* 仿函数没有固定写法，非常灵活

```c++
#include<iostrean>
#include<string>
using namespace std;

class MyPrint{
public:
    void operator()(string test){
        cout << test << endl;
    }
}
int main(){
    MyPrint myPrint;
    myPrint("Hello World");//Hello World
    // 匿名函数对象
    cout<< MyAdd()('Hello C++ !') << endl;// Hello C++ !
    system("pause");
    return 0;
}
```