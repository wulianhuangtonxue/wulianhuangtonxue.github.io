---
layout: post
title: Leetcode每日一题（2022.3.8）
subtitle:  2005 蜡烛之间的盘子（前缀和）
date:   2022-3-8
author:  物联黄同学
header-img: img/blogimg/20220307.jpg
catalog:   true
tags:
    - Leetcode
    - C++
    - 前缀和
    - 预处理
---



# Leetcode每日一题（2022.3.8）—— 2005 蜡烛之间的盘子（前缀和）

## 前言

> 刚写了今天的每日一题，没多想就暴力模拟了一下，结果好家伙超时了，一看提交历史，发现我曾经写过，看来应该是一道周赛题。



## 题目

### 内容

> 给你一个长桌子，桌子上盘子和蜡烛排成一列。给你一个下标从 0 开始的字符串 s ，它只包含字符 '*' 和 '|' ，其中 '*' 表示一个 盘子 ，'|' 表示一支 蜡烛 。
>
> 同时给你一个下标从 0 开始的二维整数数组 queries ，其中 queries[i] = [lefti, righti] 表示 子字符串 s[lefti...righti] （包含左右端点的字符）。对于每个查询，你需要找到 子字符串中 在 两支蜡烛之间 的盘子的 数目 。如果一个盘子在 子字符串中 左边和右边 都 至少有一支蜡烛，那么这个盘子满足在 两支蜡烛之间 。
>
> 比方说，s = "||**||**|*" ，查询 [3, 8] ，表示的是子字符串 "*||**|" 。子字符串中在两支蜡烛之间的盘子数目为 2 ，子字符串中右边两个盘子在它们左边和右边 都 至少有一支蜡烛。
> 请你返回一个整数数组 answer ，其中 answer[i] 是第 i 个查询的答案。
>
> 
>
> 来源：力扣（LeetCode）
>
> 题目链接：[2055. 蜡烛之间的盘子 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/plates-between-candles/)

#### 用例

![L0ecUR.png](https://s6.jpg.cm/2022/03/08/L0ecUR.png)

![L0eUwz.png](https://s6.jpg.cm/2022/03/08/L0eUwz.png)



## 题目思路——前缀和

首先，最容易想到的方法是根据数组给的左右指针的位置去对应字符串，然后两个指针不断移动到两个指针都为蜡烛时，然后开始计算两个蜡烛间的*，也就是盘子的数量。

然后在前面也提到了，作者使用这样的方法寄了，超时。

所以需要对代码的时间复杂度作优化，观察代码，我们知道，有重复计算的地方——每次根据不同的左右指针去计算蜡烛之间的盘子，如果我们采用一种思路即可以去掉这个重复的过程。

这里参考了官方题解[蜡烛之间的盘子 - 蜡烛之间的盘子 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/plates-between-candles/solution/zha-zhu-zhi-jian-de-pan-zi-by-leetcode-s-ejst/)

就是引入前缀和的方法，先将字符串按位置计算出相应的前缀和数组，这里以盘子作为计数。然后分别遍历出从左边和从右边开始的最近蜡烛位置数组，利用这两个数组就可以找到给定位置内的最近的两个蜡烛的位置，利用这两个蜡烛的位置和前缀和数组即可计算出给定位置之间的盘子数。

### code（参考官方）

```c++
#include<iostream>
#include<vector>
#include<string>

using namespace std;

// 可以模拟暴力法不行


//class Solution 
//{
//public:
//	/// <summary>
//	/// 直接模拟
//	/// 按数组给的左右下标去定位字符串的位置
//	/// 然后利用双指针
//	/// 指针相向移动，在移动过程中先确定第一次左 右都为 |
//	/// 然后开始计算左右两个指针此时之间的 * 的数量
//	/// 小细节：指针相遇时，看是否为* 
//	/// </summary>
//	/// <param name="s"></param>
//	/// <param name="queries"></param>
//	/// <returns></returns>
//	vector<int> platesBetweenCandles(string s, vector<vector<int>>& queries) 
//	{
//		int n = queries.size();
//		vector<int> ans(n);
//		// 开始计算答案数组
//		for (int i = 0; i < n; ++i)
//		{
//			auto query = queries[i];
//			int left = query[0], right = query[1];
//			// 用于标记是否左右指针都出现过|
//			bool flag = false;
//			int a = 0;
//			while (left < right)
//			{
//				// 未到条件时
//				if (!flag)
//				{
//					// 都为 | ，条件满足
//					if (s[left] == '|' && s[right] == '|')
//					{
//						flag = true;
//						left++;
//						right--;
//					}
//					else if (s[left] == '*')
//					{
//						left++;
//					}
//					else if (s[right] == '*')
//					{
//						right--;
//					}
//				}
//				// 满足条件，开始计算之间的 * 数量
//				else
//				{
//					if (s[left] == '*')
//					{
//						a++;
//					}
//					if (s[right] == '*')
//					{
//						a++;
//					}
//					left++;
//					right--;
//				}
//			}
//			// 小细节 
//			if (left == right && flag && s[left] == '*')
//			{
//				a++;
//			}
//			ans[i] = a;
//		}
//		return ans;
//	}
//};

class Solution 
{
public:
	/// <summary>
	/// 前缀和
	/// 看完题解 我超dbq我是shabi
	///	先计算字符串的*前缀和数组
	/// 然后不断计算queries的对应的子字符串中左边和右边第一个蜡烛 |
	/// </summary>
	/// <param name="s"></param>
	/// <param name="queries"></param>
	/// <returns></returns>
	vector<int> platesBetweenCandles(string s, vector<vector<int>>& queries) 
	{
		int len = s.length();
		// 前缀和数组
		vector<int> preSum(len);
		int sum = 0;
		// 计算字符串的前缀和数组
		for (int i = 0; i < len; ++i)
		{
			if (s[i] == '*')
			{
				sum++;
			}
			preSum[i] = sum;
		}
		// 左、右指针位置数组，用于标记从两个方向字符串数组包括当前位置和之前的最近蜡烛位置
		vector<int> left(len), right(len);
		// 找到字符串中当前位置或者左边的最近指针
		for(int i = 0, l = -1; i < len; ++i)
		{
			if (s[i] == '|')
			{
				l = i;
			}
			left[i] = l;
		}
		// 找到字符串中当前位置或者右边的最近指针
		for (int i = len - 1, r = -1; i >= 0; --i)
		{
			if (s[i] == '|')
			{
				r = i;
			}
			right[i] = r;
		}

		int n = queries.size();
		vector<int> ans(n);
		for (int i = 0; i < n; ++i)
		{
			auto query = queries[i];
			// x——右边的左蜡烛
			// y——左边的右蜡烛
			int x = right[query[0]], y = left[query[1]];
			ans[i] = (x == -1 || y == -1 || x >= y) ? 0 : preSum[y] - preSum[x];
		}

		return ans;
	}
};

int main(int argc, char** argv)
{
	string s;
	cin >> s;
	int n;
	cin >> n;
	vector<vector<int>> queries(n, vector<int>(2));
	for (int i = 0; i < n; ++i)
	{
		cin >> queries[i][0] >> queries[i][1];
	}
	Solution sol;
	auto ans = sol.platesBetweenCandles(s, queries);
	n = ans.size();
	for (int i = 0; i < n; ++i)
	{
		cout << ans[i] << " ";
	}
	cout << endl;

	return 0;
}
```



