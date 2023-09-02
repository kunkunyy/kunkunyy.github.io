---
title: 科学计算导论实验一并行计算MPI 
date: 2022-05-20 19:56:43
tags: 
- 科学计算导论
categories:
- [科学计算导论实验]
---

# 实验一 并行计算MPI

## 实验目的

初步了解并行计算的概念，能够在 Windows 平台上安装 MPI 库（OpenMPI或者 MPICH2），并掌握在 Visual studio 中调用 MPI 头文件的方法；初步掌握使用MPI 编制并行程序的步骤与核心函数的意义及其参数的含义。

## 实验内容

（1）介绍并行计算消息接口 MPI 的定义、应用背景与基本概念；
（2）在教师指导下在 Windows 系统上安装 MPI，（鼓励学生在 Linux 操作系统上安装以及完成实验）；
（3）教师讲解 MPI 程序编制的基本步骤，演示基于 MPI 的两个演示程序；
（4）在教师指导下，学生完成基于 MPI 的两个演示程序（鼓励学生添加入其他基于 MPI 的功能），完成相应的实验报告。

## 实验要求

（1）独立完成两个演示程序。
（2）独立完成实验报告，给出在操作系统上安装 MPI 的详细过程，以及实现的代码，着重分析MPI并行程序的优势以及可能的应用领域、普通C/C++程序与 MPI 程序的区别，在实验报告中应给出程序运行的结果截图。

## 实验过程

* 安装MPI详细过程：
* 在Visual Studio2019中配置MPI环境

{%asset_img pic1.png # tu1%}
{%asset_img pic2.png # tu1%}
{%asset_img pic3.png # tu1%}
{%asset_img pic4.png # tu1%}
{%asset_img pic5.png # tu1%}
{%asset_img pic6.png # tu1%}
{%asset_img pic7.png # tu1%}
{%asset_img pic8.png # tu1%}
{%asset_img pic9.png # tu1%}
{%asset_img pic10.png # tu1%}

* 将MPI加入到环境变量中：

{%asset_img pic11.png # tu1%}

## 实验结果

### 演示程序一

```c++
#include <mpi.h>
int main(int argc, char* argv[])
{
	int myrank, nprocs;
	// 初始化 MPI 环境
	MPI_Init(&argc, &argv);
	// 获取当前进程在通信器 MPI_COMM_WORLD 中的进程号
	MPI_Comm_size(MPI_COMM_WORLD, &nprocs);
	MPI_Comm_rank(MPI_COMM_WORLD, &myrank);
	printf("Hellow, world! %dth of totalTaskNum = %d\n", myrank, nprocs);
	MPI_Finalize();
	return 0;
}
```

{%asset_img pic12.png # tu1%}

### 演示程序二

```c++
#include "mpi.h"
#include <stdio.h>
#include <windows.h>
int main()
{
	int myrank, nprocs, name_len, flag;
	double start_time, end_time;
	char host_name[128];
	MPI_Initialized(&flag);
	printf("flag:%d/n", flag);
	MPI_Init(0, 0);//用来初始化MPI运行环境，建立多个MPI进程之间的联系
	//用来标识各个MPI进程的，给出调用该函数的进程的进程号
	//MPI_COMM_WORLD:这个进程组是MPI实现预先定义好的进程组，指的是全部MPI进程所在的进程组。
	//&rank返回调用进程中的标识号。
	MPI_Comm_rank(MPI_COMM_WORLD, &myrank);
	//用来标识对应进程组中有多少个进程
	//MPI_Comm类型的通信域，标识參与计算的MPI进程组。
	// &size返回对应进程组中的进程数。
	MPI_Comm_size(MPI_COMM_WORLD, &nprocs);
	//用于获得计算机名，并存放在processor_name中，长度为namelen。
	MPI_Get_processor_name(host_name, &name_len);
	if (myrank == 0)
	{
		//MPI_Wtick则返回MPI_Wtime结果的精度
		printf("Precision of MPI_WTIME(): %f.\n", MPI_Wtick());
		printf("Host Name:%s\n", host_name);
	}
	//MPI_Wtime返回一个双精度数，表示从过去某点的时刻到当前时刻所消耗的时间秒数
	start_time = MPI_Wtime();
	Sleep(myrank * 3);
	end_time = MPI_Wtime();
	printf("myrank: %d. I have slept %f seconds.\n", myrank, end_time-start_time);
	MPI_Finalize(); //结束MPI运行环境
	return 0;
}
```

{%asset_img pic13.png # tu1%}

## 实验分析

1.MPI 并行程序的优势：
* 无论硬件是否共享内存空间，都可以使用。
* 每个线程有自己的内存和变量，这样不用担心冲突问题
* 其并行粒度为进程级；存储方式为分布式存储；
* 可扩展性好。
2.MPI可能的应用领域
* CFD里面的粒子算法SPH
* 气候模拟，按照网格划分后用MPI
3.普通 C/C++程序与 MPI 程序的区别
* MPI程序是基于消息传递的并行程序。消息传递指的是并行运行的各个进程具有自己独立的堆栈和代码段，作为互不相关的多个程序独立运行，进程之间的信息交互全然通过显示地调用通信函数来完毕。
* 而C/C++程序通常是一个串行程序，C++支持多线程运行。
