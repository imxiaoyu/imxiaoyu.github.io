---
title: 'C++、CPLEX学习总结'
date: 2021-04-21 13:08:54
tags:
	- 科研
	- 软件使用
---

# 一、C++ CPLEX使用准备

## 1.首先需要VS+CPLEX安装好

>如果CPLEX安装包没找到可以邮件联系我发给你。


<!-- more -->

## 2.在VS中导入CPLEX安装包

>网上有很多教程可以搜一下设置一下
>**我遇到的问题：**我当时设置完了之后发现还是不能用，显示找不到包，后来发现是c盘系统文件下面没有cplex的一个文件，需要去复试一下，如果有同样问题的可以去复制一下。

# 二、C++下CPLEX的使用

>这是我们需要主要说的地方，我之前没有用过CPLEX也没有写规划的经验，所以一开始比较头疼这个东西，后来发现还是挺简单的，下面进入正文：

## 1.多维变量的创建

>IloNumVarArray类，只能定义一维的数组，而这里的模型中需要用到三维和二维的变量。Cplex C++ 接口提供了一个lloArray类模版，我们可以利用此类模版定义多维数组。

比如**二维变量数组**，在头文件中做如下定义：

	typedef IloArray<IloNumVarArray> NumVarMatrix;

接下来，便可以用NumVarMatrix定义二维变量。

	NumVarMatrix s(env, v_num);
	for (IloInt k = 0; k < v_num; k++)
	{
		s[k] = IloNumVarArray(env, c_num + 2);
		for (IloInt i = 0; i < c_num + 2; i++)
		{
			s[k][i] = IloNumVar(env, ILOINT);
		}
	}

**三维数组**也是同样的方法：

	typedef IloArray<IloNumVarArray> NumVarMatrix; 
	typedef IloArray<NumVarMatrix> NumVar3Matrix;

同样的，你如果想使用别的类型，比如整形，定义方法也是相同的：

	typedef IloArray<IloIntVarArray> IntVarMatrix;
	typedef IloArray<IntVarMatrix> IntVar3Matrix;

## 2.创建模型

	IloEnv env;
	IloModel model(env);

## 3.加入目标函数

	IloExpr obj(env);
	for (;;) {
		for (;;) {
			for (;;) {
				obj += ……;
			}
		}
	}
	model.add(IloMinimize(env, obj));

## 4.加入约束

	model.add(……)
## 5.创建求解对象

	IloCplex cplex(model);
	cplex.solve()；

	env.out() << "Solution status = " << cplex.getStatus() << endl;//输出是否有解
	env.out() << "Solution value = " << cplex.getObjValue() << endl;//输出目标函数值

## 6.得到解中固定的某个值

	c = cplex.getValue(……);//括号里面写自己定义的Ilo模式的东西

# 三、一个例子：C++调用cplex求解VRPTW完整代码

## 1.代码：

	#include <ilcplex/ilocplex.h>
	#include <stdio.h>
	#include <cstdlib>
	#include <iostream>
	typedef IloArray<IloIntVarArray> IntVarMatrix;  
	typedef IloArray<IloNumVarArray> NumVarMatrix;  
	typedef IloArray<NumVarMatrix> NumVar3Matrix; 
	typedef IloArray<IloIntArray> IntMatrix;  
	typedef IloArray<IloNumArray> NumMatrix;   
	typedef IloArray<NumMatrix> Num3Matrix;    
	
	ILOSTLBEGIN //#define ILOSTLBEGIN using namespace std;
	
	int main(int argc, char **argv)   
	{
		IloEnv env;
	
		IloModel model(env);
	
		const int M = 10000;
		const int v_num = 3, capacity = 500, c_num = 25;
		float xcord[c_num + 2], ycord[c_num + 2], t_service[c_num + 2];
		int demand[c_num + 2], lbtw[c_num + 2], ubtw[c_num + 2];
	
		//从txt文件中读取客户信息_____x,ycord，demand，tw，time_service
		ifstream inn;
		inn.open("vrptw.txt");
		if (inn)
		{
			for (int j = 0; j < c_num + 2; j++) {
				inn >> xcord[j], inn >> ycord[j], inn >> demand[j], inn >> lbtw[j], inn >> ubtw[j], inn >> t_service[j];
			}
		}
		inn.close();
	
		float distance[c_num + 2][c_num + 2], time[c_num + 2][c_num + 2];
	
		//数据初始化，计算客户之间的距离以及访问时长
		for (int i = 0; i < c_num + 2; i++) {
			for (int j = 0; j < c_num + 2; j++) {
				if (i == j) {
					distance[i][j] = 0;
					time[i][j] = 0;
				}
				else{
					distance[i][j] = floor(sqrt((xcord[i] - xcord[j]) * (xcord[i] - xcord[j]) + (ycord[i] - ycord[j]) * (ycord[i] - ycord[j]))*10)/10;
					time[i][j] = distance[i][j] + t_service[i];
				}	
			}
		}
	
		//三维决策变量x[k][i][j]，1 if a vehicle drives directly from customer i to customer j
		NumVar3Matrix x(env, v_num);
		for (IloInt k = 0; k < v_num; k++)
		{
			x[k] = NumVarMatrix(env, c_num + 2);
			for (IloInt i = 0; i < c_num + 2; i++)
			{
				x[k][i] = IloNumVarArray(env, c_num + 2);
				for (IloInt j = 0; j < c_num + 2; j++) {
					x[k][i][j] = IloNumVar(env, 0, 1, ILOBOOL);
				}
			}
		}
	
		//二维决策变量s[k][i],the time a vehicle starts to service a customer
		NumVarMatrix s(env, v_num);
		for (IloInt k = 0; k < v_num; k++)
		{
			s[k] = IloNumVarArray(env, c_num + 2);
			for (IloInt i = 0; i < c_num + 2; i++)
			{
				s[k][i] = IloNumVar(env, ILOINT);
			}
		}
	
		//约束1 对角元素为0
		for (int k = 0; k < v_num; k++) {
			for (int i = 0; i < c_num + 2; i++) {
				model.add(x[k][i][i] == 0);
			}
		}
	
		//约束2 Each customer is visited exactly once
		IloExpr sum_xkij(env);
		for (int i = 1; i < c_num + 1; i++) {
			sum_xkij.clear();
			for (int k = 0; k < v_num; k++) {
				for (int j = 0; j < c_num+2; j++) {
					sum_xkij += (x[k][i][j]);
				}
			}
			model.add(sum_xkij == 1);
		}
	
		//约束3 A vehicle can only be loaded up to it's capacity
		IloExpr sum_demand(env);
		for (int k = 0; k < v_num; k++) {
			sum_demand.clear();
			for (int i = 1; i < c_num+1; i++) {
				for (int j = 0; j < c_num + 2; j++) {
					sum_demand += (demand[i] * x[k][i][j]);
				}
			}
			model.add(sum_demand <= capacity);
		}
	
		//约束4 Each vehicle must leave the depot 0
		IloExpr sum_xk0j(env);
		for (int k = 0; k < v_num; k++) {
			sum_xk0j.clear();
			for (int j = 0; j < c_num + 2; j++) {
				sum_xk0j += (x[k][0][j]);
			}
			model.add(sum_xk0j == 1);
		}
	
		//约束5 After a vehicle arrives at a customer it has to leave for another destination
		IloExpr sumxi(env), sumxj(env);
		for (int h = 1; h < c_num + 1; h++) {
			for (int k = 0; k < v_num; k++) {
				sumxi.clear(); sumxj.clear();
				for (int i = 0; i < c_num + 2; i++) {
					sumxi += (x[k][i][h]);
					sumxj += (x[k][h][i]);
				}
				model.add(sumxi - sumxj == 0);
			}
		}
	
		//约束6 All vehicles must arrive at the depot n + 1
		IloExpr sumxki(env);
		for (int k = 0; k < v_num; k++) {
			sumxki.clear();
			for (int i = 0; i < c_num + 2; i++) {
				sumxki += (x[k][i][c_num + 1]);
			}
			model.add(sumxki == 1);
		}
	
		//约束7 The time windows are observed
		for (int i = 0; i < c_num + 2; i++) {
			for (int k = 0; k < v_num; k++) {
				model.add(s[k][i] <= ubtw[i]);
				model.add(s[k][i] >= lbtw[i]);
			}
		}
	
		//约束8 From depot departs a number of vehicles equal to or smaller than v
		IloExpr s1(env);
		for (int k = 0; k < v_num; k++) {
			for (int j = 0; j < c_num + 2; j++) {
				s1 += (x[k][0][j]);
			}
		}
		model.add(s1 <= v_num);
	
		//约束9 Vehicle departure time from a customer and its immediate successor
		for (int i = 0; i < c_num + 2; i++) {
			for (int j = 0; j < c_num + 2; j++) {
				for (int k = 0; k < v_num; k++) {
					model.add(s[k][i] + time[i][j] - M * (1 - x[k][i][j]) <= s[k][j]);
				}
			}
		}
	
		//目标函数
		IloExpr obj(env);
		for (int k = 0; k < v_num; k++) {
			for (int i = 0; i < c_num + 2; i++) {
				for (int j = 0; j < c_num + 2; j++) {
					obj += distance[i][j] * x[k][i][j];
				}
			}
		}
		model.add(IloMinimize(env, obj));
	
		//创建求解对象
		IloCplex cplex(model);
	
		//处理异常
		cout << "begin to solve." << endl;
	
		try
		{
			if (!cplex.solve())
			{
				cout << "求解失败" << endl;
				if ((cplex.getStatus() == IloAlgorithm::Infeasible) ||
					(cplex.getStatus() == IloAlgorithm::InfeasibleOrUnbounded))
				{
					cout << endl << "No solution - starting Conflict refinement" << endl;
				}
	
				env.error() << "Failed to optimize LP." << endl;
				throw(-1);
			}
			cout << "异常处理完毕" << endl;
			
			env.out() << "Solution status = " << cplex.getStatus() << endl;
			env.out() << "Solution value = " << cplex.getObjValue() << endl;
		}
	
		catch (IloException& e)
		{
			cerr << "Concert exception caught: " << e << endl;
			//save results
		}
		catch (...)
		{
			cerr << "Unknown exception caught" << endl;
		}
	
		//输出VRP路线
		cout << "Solutions: " << endl;
		for (int k = 0; k < v_num; k++) 
		{
			cout << "vehicle " << k + 1 << " :" << endl;
			for (int i = 0; i <= c_num + 1; i++) 
			{
				for (int j = 0; j <= c_num + 1; j++) 
				{
					IloInt c = cplex.getValue(x[k][i][j]);
					if (c == 1) {
						if (i == 0) cout << i << " --> " << j;
						else if (i != 0) cout << " --> " << j;
						i = j - 1;
						break;
					}
					if (j == c_num + 1) { cout << endl; break; }
				}
			}
		}
	
		env.end();
		//return 0;
		system("PAUSE");
		return EXIT_SUCCESS;
	}

## 2.vrptw.txt 输入格式

小算例：25个客户，3辆车，载重上限500。

	40	50	0	0	1236	0
	45	68	10	912	967	90
	45	70	30	825	870	90
	42	66	10	65	146	90
	42	68	10	727	782	90
	42	65	10	15	67	90
	40	69	20	621	702	90
	40	66	20	170	225	90
	38	68	20	255	324	90
	38	70	10	534	605	90
	35	66	10	357	410	90
	35	69	10	448	505	90
	25	85	20	652	721	90
	22	75	30	30	92	90
	22	85	10	567	620	90
	20	80	40	384	429	90
	20	85	40	475	528	90
	18	75	20	99	148	90
	15	75	20	179	254	90
	15	80	10	278	345	90
	30	50	10	10	73	90
	30	52	20	914	965	90
	28	52	20	812	883	90
	28	55	10	732	777	90
	25	50	10	65	144	90
	25	52	40	169	224	90
	40	50	0	0	1236	0

## 引用自

github上的一个cplex opl代码
https://github.com/afurculita/cplex-vrptw-implementation