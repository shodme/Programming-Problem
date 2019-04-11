LeetCode
---

**说明**
---
- 以下的代码实现都是基于C++

**Index**
---
- [1. Two Sum](#1-Two-Sum)
- [2. Add Two Numbers](#2-Add-Two-Numbers)
- []
- []
- []



## 1. Two Sum
> [Two Sum](https://leetcode.com/problems/two-sum/)

**题目描述**
```
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.
```

- 示例
  ```
  Given nums = [2, 7, 11, 15], target = 9,

  Because nums[0] + nums[1] = 2 + 7 = 9,
  
  return [0, 1].
  ```

**思路1**
- 对数组nums进行遍历，第一次从中选出一个数nums[i]，然后对数组nums进行第二次遍历nums[j]，选出第二个数，当
nums[i]+nums[j]的和等于target的时且i不等于j，[i j]即为所求。

**代码**
```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> result(2);
        for(int i=0;i<nums.size();i++)
        {
            for(int j=i+1;j<nums.size();j++)
            {
                if(nums[i]+nums[j]==target&&i!=j)
                {
                    result[0] = i;
                    result[1] = j;
                    return result;
                }
            }
        }
        return result;
    }
};
```

**思路2**
- 思路1的问题在于需要对数组nums进行两次循环，时间复杂度为O(n^2)；在思路2中，使用哈希表的思想，
开辟一个哈希表map，对数组进行遍历，将数组中的元素及对应的坐标位置以键值对的形式存入哈希表map中，每次在map
中查找target-nums[i]的键值对是否存在，如果存在即可将。因为在哈希表中查找时间复杂度为O(1)，遍历一遍数组nums的
时间复杂度为O(n)，所以思路2速度更快，缺点是需要开辟哈希表map而占用O(n)的空间复杂度。

**代码**
```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> result(2);
        map<int, int> map;
        for(int i=0;i<nums.size();i++)
        {
            if(map.count(target-nums[i])>0)
            {
                result[1] = i;
                result[0] = map[target-nums[i]];
                return result;
            }
            map[nums[i]] = i;
        }
        return result;
    }
};
```

## 2. Add Two Numbers
> [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

**题目描述**
```
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.
```

- 示例
  ```	
  Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
  
  Output: 7 -> 0 -> 8
  
  Explanation: 342 + 465 = 807.
  ```
**思路**
