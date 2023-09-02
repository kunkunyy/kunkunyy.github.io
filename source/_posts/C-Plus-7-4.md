---
title: C++学习笔记（七）类和对象——友元
date: 2022-04-11 21:52:25
tags:
- C++学习
categories:
- [C++学习笔记]
- [C++面向对象]
---

* 目的：让一个函数或者类访问另一个类中私有成员
* 关键字：friend
* 实现：
    * 全局函数做友元
    * 类做友元
    * 成员函数做友元

# 全局函数做友元

```c++
#include<iostrean>
#include<string>
using namespace std;
class Building{
    // 全局函数作为友元
    friend void GoodGay(Building *building)
public:
    Building(){
        m_livingroom = "客厅";
        m_bedroom = "卧室";
    }
public:
    string m_livingroom;
private:
    string m_bedroom;
}
void GoodGay(Building *building){
    cout << building->m_livingroom << endl;
    // 访问私有变量
    cout << building->m_bedroom << endl;
}
int main(){
    Building building;
    GoodGay(&building);
    system("pause");
    return 0;
}
```

# 类做友元

```c++
#include<iostrean>
#include<string>
using namespace std;
class GoodGay{
public:
    GoodGay();
    Building *building;
    void visit(); // 该函数访问Building中的属性
}
class Building{
    friend class GoodGay;
public:
    Building();
public:
    string m_livingroom;
private:
    string m_bedroom;
}
// 类外写成员函数
Building::Building(){
    m_livingroom = "客厅";
    m_bedroom = "卧室";
}
GoodGay::GoodGay(){
    building = new Building();
}
GoodGay::void visit(){
    cout <<building->m_livingroom <<endl;
    cout <<building->m_bedroom <<endl;
}
int main(){
    GoodGay gg;
    gg.visit();
    system("pause");
    return 0;
}
```

# 成员函数做友元

```c++
#include<iostrean>
#include<string>
using namespace std;
class GoodGay{
public:
    GoodGay();
    void visit(); // 该函数访问Building中的属性
    void visit2();
private:
    Building *building;
}
class Building{
    // GoodGay的成员函数作为友元，使得visit可以访问到m_bedroom
    friend void GoodGay::visit();
public:
    Building();
public:
    string m_livingroom;
private:
    string m_bedroom;
}
// 类外写成员函数
Building::Building(){
    m_livingroom = "客厅";
    m_bedroom = "卧室";
}
GoodGay::GoodGay(){
    building = new Building();
}
GoodGay::void visit(){
    cout <<building->m_livingroom <<endl;
    cout <<building->m_bedroom <<endl;
}
GoodGay::void visit2(){
    cout <<building->m_livingroom <<endl;
}
int main(){
    GoodGay gg;
    gg.visit();
    system("pause");
    return 0;
}
```