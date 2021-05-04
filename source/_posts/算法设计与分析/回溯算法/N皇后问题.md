---
title: N皇后问题
categories:
  - 算法设计与分析
  - 回溯算法
tags:
  - n皇后
cover: false
abbrlink: f7749ae2
date: 2020-05-18 16:08:55
---

## 问题
在`N∗N`的方格棋盘需要放置N个皇后，使得它们不相互攻击，即任意2个皇后不允许处在同一排，同一列，也不允许处在与棋盘边框成45角的斜线上。对于给定的N，请输出有多少种合法的放置方法。
## 输入
多组数据，每行一个正整数N，表示棋盘和皇后的数量。
> 0 ≤ N ≤ 10
## 输出
每组数据输出一行一个正整数，表示对应输入行的皇后的不同放置数量。
## 思路
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/8q_solved.png" width=300 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">8皇后问题</div>
</center>

从第1行开始，假设皇后列坐标，然后检查是否和已经排好的皇后坐标有
- 列冲突
- 右对角线冲突
- 左对角线冲突

假设成立，继续下一行：行+1；
设不成立，继续下一列：列+1。
## 代码
```c++
#include<iostream>
using namespace std;

//queue数组记录每行的皇后列坐标
int queue[20], sum = 0;
//打印合法的放置皇后坐标
void show(int row) {
	for (int i = 0; i < row; i++) {
		cout << i << ':' << queue[i] << ' ';
	}
	cout << endl;
	sum++;
}
//检查前row行和第row行的皇后列坐标是否冲突
bool check(int row) {
	for (int i = 0; i < row; i++) {
		if (queue[i] == queue[row] || //同列冲突
			queue[i] - queue[row] == row - i || //右对角线冲突
			queue[row] - queue[i] == row - i) //左对角线冲突
			return true;
	}
	return false;
}
void place(int row, int N) {
	for (int col = 0; col < N; col++) {
		queue[row] = col;           //假设第row行的皇后在第col列
		if (!check(row))            //假设成立，无冲突
		{
			if (row == N - 1)       //如果是最后一行，打印皇后放置坐标
				show(row);
			else                    //否则递归row + 1
				place(row + 1, N);
		}
	}
}
int main() {
	int N;
	while (cin >> N) {
		place(0, N);
		cout << sum << endl;
	}
	return 0;
}
```