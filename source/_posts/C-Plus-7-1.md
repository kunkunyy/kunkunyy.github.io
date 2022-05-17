---
title: C++学习笔记（七）类和对象——封装
date: 2022-04-11 21:19:53
tags:
- C++学习
categories:
- [C++学习笔记]
- [C++面向对象]
---

# 封装的意义

* 将属性和行为作为一个整体来表现生活中的事物。
* 将属性和行为加以权限和控制。

* 设计一个圆类，求圆的周长。

```c++
#include<iostream>
using namespace std;
#define PI 3.14;
class Circle{
    // 访问权限
public:
    // 属性
    int m_r;
    //行为
    double calculate2C(){
        return 2 * PI * m_r;
    }
}

int main(){
    Circle c1;
    c1.m_r = 10;
    cout<< "圆的周长为"<< c1.calculate() <<endl;// 圆的周长为26.8
    return 0;
}
```

* 设计一个学生类，属性有姓名和学号，可以给姓名和学号赋值，可以显示学生的姓名和学号。

```c++
#include<iostream>
#include<string>
using namespace std;

class Student{
    public:
    string name;
    string number;
    string show(){
        return "学生姓名为：" + name + "学生学号为：" + number;
    }
}

int main(){
    Student stu1;
    stu1.name = "张三";
    stu1.number = "100011223";
    cout<< stu1.show() <<endl;
    return 0;
}
```

* **访问权限有三种：**
    * public：公共权限
    * protected：保护权限
    * private：私有权限

# struct与class区别

* struct默认权限为公有；
* class默认权限为私有。

# 将成员属性设为私有

* 优点：
    * 将所有成员属性设置为私有，可以自己控制读写权限。
    * 对于写权限，我们可以检测数据的有效性。

```c++
#include<iostream>
#include<string>
using namespace std;
class Person{
public:
    void setName(string name){
        m_name = name;
    }
    string getName(){
        return m_name;
    }
    int getAge(){
        m_age = 18;
        return m_age;
    }
    void setAge(int age){
        if(age < 0 || age > 150){
            cout << "输入的年龄不合法！！！" << endl;
            return ;
        }
        m_age = age;
    }
    void setLover(string lover){
        m_lover = lover;
    }

private:
    string m_name;
    int m_age;
    string m_lover
}

int main(){
    Person p;
    p.setName("张三");
    p.serLover("小花");
    cout<< p.getName() <<endl;// 张三
    cout<< p.getAge() <<endl;// 18
    return 0;
}
```