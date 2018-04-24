---
layout: post
title: 'Two Sum'
author: 'James Peng'
tags: ['leetcode C# ']
---

## Question ##

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution.

Example:
    Given nums = [2, 7, 11, 15], target = 9,
    
    Because nums[0] + nums[1] = 2 + 7 = 9,
    return [0, 1].

https://leetcode.com/problems/two-sum/


## Submit Solution ##

~~~csharp
public class Solution {
    public int[] TwoSum(int[] nums, int target) 
    {
           int[] result = null;
            for (int i = 0; i < nums.Length; i++)
            {
                for (int j = i+1; j < nums.Length; j++)
                {
                    int a = nums[i] + nums[j];
                    if (a.Equals(target))
                    {
                        result = new int[2];
                        result[0] = i;
                        result[1] = j;
                        return result;
                    }
                }
            }
            return result;
    }
}
~~~

![](..\images\2016-06-10-LeetCode_TwoSum\oja2UKK.png)


----------

參考:

- https://leetcode.com/
