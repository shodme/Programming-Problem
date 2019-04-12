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
- 如果链表1和2的长度一致的话，直接相加，如果相加后有进位的话，再增加一位长度；若链表1和2长度不一致的话，两个链表相同长度部分相加，然后将较长的链表多出部分接在后面，如果相加后有进位的话，下一位需要继续加1，直到没有进位为止。一共可以分为如下几种情况：

（1）长度相等-->相加无进位-->返回

（2）长度相等-->相加有进位-->新增下一个节点，值为1-->返回

（3）长度不等-->相加无进位-->长度相等部分相加，多出部分接在后面-->返回

（4）长度不等-->相加有进位-->长度相等部分相加，多出部分接在后面-->多出的下一位继续加1，直到无进位-->返回

**代码**
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        
        ListNode* H = new ListNode(0);
	    ListNode* Head = H;
        
        // 进位标志
        int flag = 0;
        
        int temp = 0;
        
        while(l1&&l2)
        {
            ListNode* Node = new ListNode(0);
            Node->next = NULL;
            
            temp = l1->val + l2->val + flag;
            // 加完flag后重置
            flag = 0;
            Node->val = temp % 10;
            if(temp>=10)
            {
                flag = 1;
            }
            H->next = Node;
            H = H->next;
            l1 = l1->next;
            l2 = l2->next;
        }
        // 考虑有进位
        if(flag&&!l2&&!l1)
        {
            ListNode* Node = new ListNode(1);
            Node->next = NULL;
            H->next = Node;            
        }
           
        if(l2)
        {
            H->next = l2;
            // 考虑有进位
            if(flag)
            {
                while(l2)
                {
                    temp = l2->val + flag;
                    l2->val = temp % 10;
                    flag = 0;
                    if(temp>=10)
                    {
                        flag = 1;
                    }
                    l2 = l2->next;
                    H = H->next;
                }
                if(flag)
                {
                    ListNode* Node = new ListNode(1);
                    Node->next = NULL;
                    H->next = Node;
                }             
            }
        }
        
        if(l1)
        {
            H->next = l1;
            // 考虑有进位
            if(flag)
            {
                while(l1)
                {
                    temp = l1->val + flag;
                    l1->val = temp % 10;
                    flag = 0;
                    if(temp>=10)
                    {
                        flag = 1;
                    }
                    l1 = l1->next;
                    H = H->next;
                }
                if(flag)
                {
                    ListNode* Node = new ListNode(1);
                    Node->next = NULL;
                    H->next = Node;
                }             
            }
        }
        return Head->next;
    }
};
```

