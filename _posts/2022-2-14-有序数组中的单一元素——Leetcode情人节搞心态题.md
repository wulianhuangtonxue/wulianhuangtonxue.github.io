---
layout:   post
title:    有序数组中的单一元素
subtitle: Leetcode情人节搞心态题
date:     2022-2-14
author:   物联黄同学
header-img: img/20220213.jpg
catalog:  true
tags:
    - Leectode
    - 每日一题
    - C++
    - 二分查找
---

# 有序数组中的单一元素——Leetcode情人节搞心态题

## 题目

> 给你一个仅由整数组成的有序数组，其中每个元素都会出现两次，唯有一个数只会出现一次。
>
> 请你找出并返回只出现一次的那个数。
>
> 你设计的解决方案必须满足 O(log n) 时间复杂度和 O(1) 空间复杂度。
>
>  示例 1:
>
> 输入: nums = [1,1,2,3,3,4,4,8,8]
> 输出: 2
> 示例 2:
>
> 输入: nums =  [3,3,7,7,10,11,11]
> 输出: 10
>
> **提示:**
>
> - `1 <= nums.length <= 105`
> - `0 <= nums[i] <= 105`
>
> 附上题目链接：[540. 有序数组中的单一元素 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/single-element-in-a-sorted-array/)



## 个人感受

yysy，在我看懂题目之后，真的感觉有被搞到，很佩服leetcode出题人，几千道题目里面冷食给他找出一道来爆杀🐕，情人节专门搞我的心态，咋得别人都是成双成对是吧，就哥们我独一无二，搁这敲键盘是吧。焯！



## 题解

没啥好说的，二分查找判断奇偶下标就完事了

因为只有一个唯一且按序排列，所以可以假设唯一的元素的下标为x，其左边的下标y，如果nums[y] = nums[y+1]，则y为偶数, 同样的，对于右边的, 如果nums[y] = nums[y+1]，则y为奇数。
基于此，可以设置二分查找。

> 我在leetcode的题解，需要的可以去看一下[二分查找 - 有序数组中的单一元素 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/single-element-in-a-sorted-array/solution/er-fen-cha-zhao-by-killing-o-3eg5/)



### code（完整版）

```c++
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

class Solution 
{
public:
    /// <summary>
    /// 二分查找
    /// 由于只有一个唯一且按序排
    /// 所以假设唯一的下标为x，其左边的下标y，如果nums[y] = nums[y+1]，则y为偶数
    /// 同样的，对于右边的, 如果nums[y] = nums[y+1]，则y为奇数
    /// 根据上诉设置二分查找
    /// 初始时，左边界取0，右边界取n-1，n为数组元素个数
    /// 每次查找mid取左边界和右边界的中间，分奇偶判断
    /// 当mid为奇数时，判断nums[mid] == nums[mid-1]
    /// 当mid为偶数时，判断nums[mid] == nums[mid+1]
    /// 上述成立一个则 mid < x， 反之mid > x
    /// </summary>
    /// <param name="nums"></param>
    /// <returns></returns>
    int singleNonDuplicate(vector<int>& nums) 
    {
        int n = nums.size();
        // 特殊情况，只有一个数时
        if (n == 1)
        {
            return nums[0];
        }
        int left = 0, right = n - 1;
        // 二分查找
        while (left < right)
        {
            int mid = left + (right - left) / 2;
            if (mid % 2 == 0 && nums[mid] == nums[mid + 1] ||
                mid % 2 == 1 && nums[mid] == nums[mid - 1])
            {
                left = mid + 1;
            }
            else
            {
                right = mid;
            }
        }
        return nums[left];
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
    // 确保一下自己输入的数据有序
    sort(nums.begin(), nums.end());
    Solution sol;
    cout << sol.singleNonDuplicate(nums) << endl;

	return 0;
}
```



## 后话

> 今天是被爆杀的一天。。。
