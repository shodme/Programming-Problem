剑指Offer
---

**说明**
---
- 以下的代码实现都是基于C++

**Index**
---
- [3.1 数组中重复的数字](#31-数组中重复的数字)
- []
- []
- []



## 31. 数组中重复的数字
> [数组中重复的数字](https://www.nowcoder.com/practice/623a5ac0ea5b4e5f95552655361ae0a8?tpId=13&tqId=11203&tPage=3&rp=3&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

**题目描述**
```
在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。
请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。
```

- 示例
  ```
  Given nums = [2, 3, 1, 0, 2, 5, 3]
  
  return 2
  ```

**思路1**
- 可以使用哈希表方法，开辟一个长度为n-1的哈希表用于统计每个数出现次数，每遍历数组中的一个数，哈希表中对应位置加1，
当某个位置次数大于1时，返回这个数。这种方法的时间复杂度为O(n)，空间复杂度为O(n)。

**代码**
```C++
class Solution {
public:
    // Parameters:
    //        numbers:     an array of integers
    //        length:      the length of array numbers
    //        duplication: (Output) the duplicated number in the array number
    // Return value:       true if the input is valid, and there are some duplications in the array number
    //                     otherwise false
    bool duplicate(int numbers[], int length, int* duplication) {
        if(numbers==NULL||length==0) return 0;
        int *hashmap = new int[length]();
        for(int i=0;i<length;i++)
        {
            hashmap[numbers[i]]++;
             
            if(hashmap[numbers[i]]>1)
            {
                duplication[0] = numbers[i];
                return 1;
            }
             
        }
         
        return 0;
    }
};
```

**思路2**
- 因为n个数字的范围为'0'到'n-1'，所以使用一种类似选择排序的方法，通过一次遍历将每个数交换到排序后的位置上去，如果该位置已存在相同数字，
那么该数就是重复的。尽管代码中有一个两重循环，但是每个数字一次交换就可以到自己对应的位置上，因此总的时间复杂度为O(n)，因为所有操作再输入
数组中操作，所以空间复杂度为O(1)。

**代码**
```C++
class Solution {
public:
    // Parameters:
    //        numbers:     an array of integers
    //        length:      the length of array numbers
    //        duplication: (Output) the duplicated number in the array number
    // Return value:       true if the input is valid, and there are some duplications in the array number
    //                     otherwise false
    bool duplicate(int numbers[], int length, int* duplication) {
        if(numbers==nullptr||length<=0)    return 0;
        for(int i=0;i<length;i++)
        {
            if(numbers[i]>length-1||numbers[i]<0)    return 0;
        }
        for(int i=0;i<length;i++)
        {
            while(numbers[i]!=i)
            {
                if(numbers[i]==numbers[numbers[i]])
                {
                    *duplication = numbers[i];
                    return 1;
                }         
                // swap numbers[i] and numbers[numbers[i]]
                int temp = numbers[numbers[i]];
                numbers[numbers[i]] = numbers[i];
                numbers[i] = temp;
            }
            
        }
        return 0;
    }
};
```

