---
layout:     post   				    # 使用的布局（不需要改）
title:      Leetcode刷题知 				# 标题 
subtitle:   哈希表可以遍历 #副标题
date:       2022-1-30 				# 时间
author:     物联黄同学 						# 作者
header-img: img/20220129.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - C++
    - leetcode
    - 哈希表
---
# Leetcode刷题知——哈希表可以遍历

> 今天的每日一题很简单，但是通过这道题，我了解到在我此前不知道的知识点：
>
> 哈希表是可以遍历的！

题目链接[884. 两句话中的不常见单词 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/uncommon-words-from-two-sentences/)

我的leetcode题解：[遍历哈希表 - 两句话中的不常见单词 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/uncommon-words-from-two-sentences/solution/bian-li-ha-xi-biao-by-killing-o-r8na/)

## 题目分析

题目要我们找到只出现过一次的单词！

注意这个一次，包括了同一个字符串中



## 思路——哈希表

我们遍历字符串，如果遇到空格或者结尾，就将单词截取出来，存进哈希表。

然后遍历哈希表，如果哈希表对应单词的次数为1，则表示其为不常见的单词，存进答案即可。



## 代码C++

```C++
#include<iostream>
#include<string>
#include<unordered_map>

using namespace std;

class Solution 
{
public:
    /// <summary>
    /// 哈希表
    /// 遍历两个字符串，将单词存进哈希表
    /// 然后遍历哈希表
    /// 如果单词在哈希表只出现一次
    /// 则表示当前字符串为不常见单词
    /// </summary>
    /// <param name="s1">字符串1</param>
    /// <param name="s2">字符串2</param>
    /// <returns>返回不常见的单词的集合，即字符串数组</returns>
    vector<string> uncommonFromSentences(string s1, string s2)
    {
        // 哈希表，存储出现过的单词次数
        unordered_map<string, int> hashmap;
        // 获取字符串长度
        int len1 = s1.size(), len2 = s2.size();
        int start = 0;
        for (int i = 0; i < len1; ++i)
        {
            char ch = s1[i];
            // 如果出现空格或者结尾处，需要分割出单词存进哈希表
            if (ch == ' ')
            {
                hashmap[s1.substr(start, i - start)]++;
                start = i + 1;
            }
            else if (i == len1 - 1)
            {
                hashmap[s1.substr(start, i - start + 1)]++;
            }
        }
        start = 0;
        vector<string> ans;
        for (int i = 0; i < len2; ++i)
        {
            char ch = s2[i];
            if (ch == ' ')
            {
                hashmap[s2.substr(start, i - start)]++;
                start = i + 1;
            }
            else if (i == len2 - 1)
            {
                hashmap[s2.substr(start, i - start + 1)]++;
            }
        }
        for (auto it = hashmap.begin(); it != hashmap.end(); ++it)
        {
            // 如果出现次数一次，则存进答案
            if (it->second == 1)
            {
                ans.push_back(it->first);
            }
        }
        return ans;
    }
};

int main(int argc)
{
    string s1, s2;
    getline(cin, s1);
    getline(cin, s2);
    Solution sol;
    auto ans = sol.uncommonFromSentences(s1, s2);
    int n = ans.size();
    for (int i = 0; i < n; ++i)
    {
        cout << ans[i] << " ";
    }
    cout << endl;

	return 0;
}
```

