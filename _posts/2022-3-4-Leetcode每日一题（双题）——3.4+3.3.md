---
layout: post
title: Leetcode每日一题(双题版)
subtitle:  3.4+3.3
date:   2022-3-4
author:  物联黄同学
header-img: img/20220303.jpg
catalog:   true
tags:
    - Leetcode
    - C++
    - 模拟
    - 数学
    - 单调栈
---

# Leetcode每日一题（双题）——3.4+3.3

## 前言

> 写了一下这两天的每日一题，了解到了一些新的知识点，做一下梳理与总结。

## 3.3 各位相加

> 给定一个非负整数 `num`，反复将各个位上的数字相加，直到结果为一位数。返回这个结果。
>
> 题目链接[258. 各位相加 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/add-digits/)

### 进阶

> 你可以不使用循环或者递归，在 `O(1)` 时间复杂度内解决这个问题吗？



### 分析与思路

首先，可以很容易想到使用两层循环嵌套的做法，最外层的循环用于判断数字是否是0到9的整数，里层的循环用于计算当前数字各位数的加和。

但是，在看到进阶时，确实被难住了。

查阅过[官方题解](https://leetcode-cn.com/problems/add-digits/solution/ge-wei-xiang-jia-by-leetcode-solution-u4kj/)后，了解到了一些新的知识点。首先，一个数字的各位数字经过不断加和的最终会得到一个0到9的数字，当然0是特殊情况，只有一开始是0才会得到，这个数字在数学上被称为自然数的**数根**。

#### 数学推导

这里先假设数字为num，它的各位数字为 ai，（i = 1， 2， 3）i表示从个位开始的第几位，即每一次加和的结果为temp
![image](https://user-images.githubusercontent.com/88485359/156730865-4072f225-134d-4e0b-ba8d-2b362bf634f8.png)

根据式子我们可以得知，因为10的i-1次方-1一定为9的倍数，所以num和temp的模9同余。重复计算，就会得到数根，所以可以得知一个**自然数的数根其实就是模9的结果**，当然这里有一些小细节。

#### 小细节

我们知道如果是九的倍数模9的结果是0，但是数根应该是9，所以需要特殊处理一下。



### code

注释里包含了基础解法

```c++
#include<iostream>

using namespace std;

class Solution 
{
public:
    /// <summary>
    /// 简单模拟
    /// 分两层循环实现
    /// 外层判断是否超过1位
    /// 里层加和各位数字
    /// </summary>
    /// <param name="num"></param>
    /// <returns></returns>
    /*int addDigits(int num) 
    {
        while (num >= 10)
        {
            int temp = num;
            num = 0;
            while (temp)
            {
                num += temp % 10;
                temp /= 10;
            }
        }
        return num;
    }*/
    // 我是真的没想到，tql，数学
   
    /// <summary>
    /// 数学解法
    /// 最终的结果是自然数的数根
    /// 原理，num 与 数根 模9同余
    /// </summary>
    /// <param name="num"></param>
    /// <returns></returns>
    int addDigits(int num)
    {
        return (num - 1) % 9 + 1;
    }
};

int main(int argc, char** argv)
{
    int num;
    cin >> num;
    Solution sol;
    cout << sol.addDigits(num) << endl;

	return 0;
}
```



## 3.4 子数组范围和

> 给你一个整数数组 nums 。nums 中，子数组的 范围 是子数组中最大元素和最小元素的差值。
>
> 返回 nums 中 所有 子数组范围的 和 。
>
> 子数组是数组中一个连续 非空 的元素序列。
>
> 题目链接：[2104. 子数组范围和 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/sum-of-subarray-ranges/)

### 进阶

> 你可以设计一种时间复杂度为 `O(n)` 的解决方案吗？

### 分析与思路

不看进阶还好，可以直接暴力模拟，不断计算子集的范围。一看进阶，确实没想出来可以O（n）的解法。

看了题解才明白，是用单调栈来做。[子数组范围和 - 子数组范围和 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/sum-of-subarray-ranges/solution/zi-shu-zu-fan-wei-he-by-leetcode-solutio-lamr/)

首先，我们可以知道，范围和其实是数组所有的子集的最大值和最小值的差值的加和，数学推导如下。

#### 数学推导

![image](https://user-images.githubusercontent.com/88485359/156731126-c38a5103-cc03-428b-b7cb-35a4edadc7a3.png)


那么如何枚举所有子数组的最大最小值呢？

假设当前枚举到的数字下标为i，数值为nums[i]，我们可以从左边和右边枚举出比该数值第一个大的数值的下标，然后我们可以利用下这些下标和i的间距就可以计算出以nums[i]为最大值子数组的数量，即间距的乘积。

把所有的乘积加和即为Maxi的加和，同理也可以得到Mini的加和。

那么问题来了，如果在遍历时需要去找两侧的值，显然不满足时间复杂度为O（n），所以这里需要引入单调栈   

我们使用单调栈去存储最值，以最小值为例，遍历数组时，对当前数值，对栈顶元素判断并执行出栈的操作，直至栈空或者栈顶元素对应的数组元素值比当前元素值小，然后将当前下表入栈，最大值同理。

#### 小细节

对于相等时，需要引入**逻辑大小**，即加入下标作为判断，这里假设有下标 i 和 j，当数值相等时，i < j 时，nums[i] 逻辑小于 nums[j]

需要先遍历两次数组，即记录两个方向的 以 nums[i] 为最大值或最小值的单侧最大子集长度。所以需要两个栈，四个数组的其实还是O（n）的空间。

这里其实就是使用了以空间换时间的思想，将时间复杂度从 n^2 降为成n。

### code 单调栈

```c++
#include<iostream>
#include<vector>
#include<stack>

using namespace std;

class Solution 
{
public:
    /// <summary>
    /// 单调栈
    /// 用栈分别存储最大值和最小值
    /// 结果其实是最大值加和 减去最小值的加和
    /// 
    /// </summary>
    /// <param name="nums"></param>
    /// <returns></returns>
    long long subArrayRanges(vector<int>& nums) 
	{
        int n = nums.size();
        vector<int> minLeft(n), minRight(n), maxLeft(n), maxRight(n);
        // 存储最大值和最小值对应的数组下标
        stack<int> minStack, maxStack;
        for (int i = 0; i < n; ++i)
        {
            while (!minStack.empty() && nums[minStack.top()] > nums[i])
            {
                minStack.pop();
            }
            minLeft[i] = minStack.empty() ? -1 : minStack.top();
            minStack.push(i);

            // 此处的 <= 其实是相等时，下标会小于当前下标，构成逻辑小
            while (!maxStack.empty() && nums[maxStack.top()] <= nums[i])
            {
                maxStack.pop();
            }
            maxLeft[i] = maxStack.empty() ? -1 : maxStack.top();
            maxStack.push(i);
        }
        // 这里应该是将两个单调栈重新构造，初始化为空栈
        minStack = stack<int>();
        maxStack = stack<int>();
        for (int i = n - 1; i >= 0; --i)
        {
            while (!minStack.empty() && nums[minStack.top()] >= nums[i])
            {
                minStack.pop();
            }
            minRight[i] = minStack.empty() ? n : minStack.top();
            minStack.push(i);

            while (!maxStack.empty() && nums[maxStack.top()] < nums[i])
            {
                maxStack.pop();
            }
            maxRight[i] = maxStack.empty() ? n : maxStack.top();
            maxStack.push(i);
        }

        long long sumMax = 0, sumMin = 0;
        for (int i = 0; i < n; i++)
        {
            // static_cast 强制类型转换，将类型转换为目标的类型, 属于隐式类型转换
            sumMax += static_cast<long long>(maxRight[i] - i) * (i - maxLeft[i]) * nums[i];
            sumMin += static_cast<long long>(minRight[i] - i) * (i - minLeft[i]) * nums[i];
        }
        return sumMax - sumMin;
    }
};

int main(int argc, char** argv)
{
    int n;
    cin >> n;
    vector<int> nums(n);
    for (int i = 0; i < n; ++i)
    {
        cin >> nums[i];
    }
    Solution sol;

    cout << sol.subArrayRanges(nums) << endl;
    	
	return 0;
}

```



## 后话

刷起来，卷起来！
