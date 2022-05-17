---
title: C++学习笔记（七）类和对象——对象的初始化和清理
date: 2022-04-11 21:29:37
tags:
- C++学习
categories:
- [C++学习笔记]
- [C++面向对象]
---

# 构造函数和析构函数

* 构造函数：用于初始化；
* 析构函数：用于清理。
* 编译器会自动调用，用于完成对象初始化和清理工作。
* 如果不提供以上两者，那么编译器会提供，但编译器提供的构造函数和析构函数是空实现。

**构造函数语法：**```类名(){}```
    * 构造函数没有返回值也不写void；
    * 函数名与类名相同；
    * 构造函数可以有参数，因此可以发生重载；
    * 程序在调用对象是会自动调用构造函数，**无需手动调用，且只会调用一次**。

**析构函数语法：**```~类名(){}```
    * 析构函数，没有返回值也不写void；
    * 函数名与类名相同，同时在名称前加上~；
    * 构造函数**不可以**有参数，因此不可以发生重载；
    * 程序在对象销毁前会自动调用析构，**无需手动调用，而且只会调用一次**。


```c++
#include<iostream>
using namespace std;
class Person{
public:
    // 构造函数
    Person(){
        cout << "构造函数已调用！" << endl;
    }
    // 析构函数
    ~Person(){
        cout << "析构函数已调用！" << endl;
    }

}
void func(){
    Person p;
    cout<< 111 << endl;
}
int main(){
    func()
    //构造函数已调用！
    // 111
    //析构函数已调用！
    return 0;
}
```

# 构造函数的分类及调用

* 两种分类方式：
    * 按参数分：有参构造和无参构造
    * 按类型分为：普通构造和拷贝构造

* 三种调用方式：
    * 括号法
    * 显示法
    * 隐式转化法

```c++
#include<iostream>
using namespace std;
class Person{
public:
    // 无参构造（默认构造函数）
    Person(){
        cout<< "无参构造函数调用" <<endl;
    }
    // 有参构造
    Person(int a){
        age = a;
        cout<< "有参构造函数调用" <<endl;
    }
    // 拷贝构造函数
    Person( const Person &p){
        // 将传入的人的所有属性都进行拷贝
        age = p.age;
        cout<< "拷贝构造函数调用" <<endl;
    }
    ~Person(){
        cout<< "析构函数调用" <<endl;
    }
    int age;
}

void test(){
    // 1、括号法
    Person p1;// 默认构造函数调用
    Person p2(10);// 调用有参构造函数
    Person p3(p2);// 调用拷贝构造函数

    cout<< p2.age <<endl;// 10
    cout<< p3.age <<endl;// 10
    /*
        Person p1();<==等价于==>void p1;
        调用默认构造函数的时候不要加()
        否则编译器会认为是一个函数的声明
    */

    // 2、显示法
    Person p4;// 默认构造函数调用
    Person p5 = Person(10);// 调用有参构造函数
    Person p6 = Person(p5);// 调用拷贝构造函数
    /*
        匿名对象：Person(10);
        当前行执行完毕后，系统会立即回收匿名对象

        不要使用拷贝构造函数来初始化匿名对象，编译器会认为重定义对象即：
        Person(p3)<==等价于==>Person p3
    */

    // 3、隐式转化法

    Person p7 = 10;// 等价于 Person p7 = Person(10)
    Person p8 = p7;// 等价于 Person p8 = Person(p7);
}
int main(){
    test()
    cout<<  <<endl;
    return 0;
}
```

# 拷贝构造函数调用时机

C++中拷贝构造函数调用时机通常有三种情况：

* 使用一个已经创建完毕的对象来初始化一个新对象。
* 值传递的方式给函数参数传值。
* 以值方式返回局部对象。

```c++
#include<iostream>
using namespace std;

class Person{
public:
    int age;
    Person(){
        cout<< "无参构造函数调用" <<endl;
    }
    Person(int a){
        age = a;
        cout<< "有参构造函数调用" <<endl;
    }
    Person( const Person &p){
        age = p.age;
        cout<< "拷贝构造函数调用" <<endl;
    }
    ~Person(){
        cout<< "析构函数调用" <<endl;
    }
}

void doWork(Person p){

}
void doWork2(){
    Person p;
    return p;
}
// 使用一个已经创建完毕的对象来初始化一个新对象。
void test01(){
    Person p1(20);
    Person p2(p1);
}
// 值传递的方式给函数参数传值。
void test02(){
    Person p;
    dowork(p);
}
// 以值方式返回局部对象。
void test03(){
    Person p3 = doWork2();
}

int main(){
    test01();
    //无参构造函数调用
    //拷贝构造函数调用
    //析构函数调用
    //析构函数调用
    test02();
    //无参构造函数调用
    //拷贝构造函数调用
    //析构函数调用
    //析构函数调用
    test03();
    //无参构造函数调用
    //拷贝构造函数调用
    //析构函数调用
    //析构函数调用
    return 0;
}
```

# 构造函数调用规则

* 如果用户定义有构造函数，c++不再提供默认无参构造，但是会提供默认拷贝构造。
* 如果用户定义拷贝构造函数，c++不会再提供其他构造函数。

# 深拷贝和浅拷贝

* 浅拷贝：简单的赋值拷贝操作。
* 深拷贝：在**堆区**重新申请空间，进行拷贝操作。
* **如果属性有在堆区开辟的，一定要自己提供给拷贝构造函数，防止浅拷贝带来的问题。**

```c++
#include<iostream>
using namespace std;
class Person{
public:
    // 无参构造（默认构造函数）
    Person(){
        cout<< "无参构造函数调用" <<endl;
    }
    // 有参构造
    Person(int a,int height){
        age = a;
        height = new int(height);
        cout<< "有参构造函数调用" <<endl;
    }
    // 拷贝构造函数
    Person( const Person &p){
        // 将传入的人的所有属性都进行拷贝
        age = p.age;
        height = new int(&p.height);
        cout<< "拷贝构造函数调用" <<endl;
    }
    ~Person(){
        // 将对去开辟的数据进行释放
        if(height != NULL){
            delete height;
            height = NULL;
        }
        cout<< "析构函数调用" <<endl;
    }
    int age;
    int *height;
}
void test01(){
    Person p1(18, 160);
    Person p2(p1);
}
int main(){
    test01();
    return 0;
}
```

# 初始化列表

语法：```构造函数(): 属性1(值1),属性2(值2)...{}```

```c++
#include<iostream>
using namespace std;
class Person{
public:
    Person(string a,int b,int c):name(a),age(b),height(c){

    }
private:
    string name;
    int age;
    int height;
}
void test01(){
    Person p1()
}
int main(){
    test01('张三'，18，185);
    return 0;
}
```

# 类对象作为类成员

* C++类中的成员可以是另一个类的对象，我们称该成员为对象成品。
* 当其他类对象作为本类成员，构造时先构造类对象，再构造自身，析构的顺序与构造顺序相反。

```c++
#include<iostream>
#include<string>
using namespace std;
class Phone{
public:
    Phone(string name){
        m_PName = name;
    }
    string m_PName;

}
class Person{
public:
    Person(string name,string pName):m_Name(name),m_Phone(pName){
        m_Name = name;
        
    }
    string m_Name;
    Phone m_Phone;

}
void test(){
    Person p("张三","HONER");
}
int main(){
    test();
    return 0;
}
```

# 静态成员

* 静态成员就是在成员变量和成员函数前加上笑键字static，称为静态成员。
* 静态成员分为：
    * 静态成员变量。
        * 所有对象共享同一份数据
        * 在编译阶段分配内存
        * 类内声明，类外初始化
    * 静态成员函数。
        * 所有对象共享同一个函数
        * 静态成员函数只能访问静态成员变量
        * 有访问权限

```c++
#include<iostream>
using namespace std;

class Person{
public:
    // 静态成员函数
    static void func(){
        cout <<"静态成员函数调用"<<endl;
    }
    static int m_A;// 类内定义
}
int Person::m_A = 0;// 类外初始化
void test(){
    // 1、通过对象访问
    Person p;
    p.func();
    // 2、通过类名访问
    Person::func();
}
int main(){
    cout<<  <<endl;
    return 0;
}
```