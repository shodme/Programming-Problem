剑指Offer
---

**说明**
---
- 以下的代码实现都是基于C++

**Index**
---
- [3.1 数组中重复的数字](#31-数组中重复的数字)
- [3.2 不修改数组找出重复数字](#32-不修改数组找出重复数字)
- [4. 二维数组中的查找](#4-二维数组中的查找)
- []



## 3.1 数组中重复的数字
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
## 3.2 不修改数组找出重复数字

**题目描述**
```
在一个长度为n+1的数组里的所有数字都在1到n的范围内，所以数组中至少有一个数字是重复的。
请找出数组中任意一个重复的数字，但不能修改输入的数组。
例如，如果输入长度为8的数组{2, 3, 5, 4, 3, 2, 6, 7}，那么对应的输出是重复的数字2或者3。
```

**思路**
- 可以和3.1中一样开辟一个哈希表，但是需要占O(n)的空间复杂度。
- 对数组进行二分查找，每次划分后都统计两边数字出现个数是否和划分大小一致，若不一致，就继续划分，直到找到那个重复数字。

**代码**
```C++
class Solution {
public:
	int duplicate(int numbers[], int length) {
		if (numbers == nullptr || length <= 0)    return 0;
		for (int i = 0; i<length; i++)
		{
			if (numbers[i]>length || numbers[i]<0)    return 0;
		}

		int begin = 1;
		int end = length - 1;
		int middle, count;
		while (begin <= end)
		{
			middle = (begin + end) / 2;
			count = countnumber(numbers, length, begin, middle);
			if (begin == end)
			{
				if (count > 1)
					return begin;
				else
					break;
			}
			if (count == middle - begin + 1)
				begin = middle + 1;
			else
				end = middle;
		}
		return 0;
	}
	int countnumber(const int* numbers, int length, int begin, int end)
	{
		int count = 0;
		for (int i = 0; i<length; i++)
		{
			if (numbers[i] >= begin&&numbers[i] <= end)
				++count;
		}
		return count;
	}
};
```

# 4. 二维数组中的查找
> [二维数组中的查找](https://www.nowcoder.com/practice/abc3fe2ce8e146608e868a70efebf62e?tpId=13&tqId=11154&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

**题目描述**
```
在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
```

**思路**
- 如果从二维数组的**左上角**开始找起，当目标数字大于左上点时，满足下一步查找条件的区域在该点的下方或者右方，这样会使得查找方向有分歧，问题变得复杂。从**右下角**开始查找同理。
- 如果从二维数组的**右上角**开始找起，当目标数字大于右上角点时，满足下一步查找的区域只会在下方，而小于右上角点的区域只会在左方，这样查找不会存在分歧，一行一列的逐步排除，可以逐步缩小查找区域。从**左下角**查找同理。

**代码**
```C++
class Solution {
public:
    bool Find(int target, vector<vector<int>> array) {
        if (array.empty())
		    return 0;
        int i = 0;
        int j = array[0].size() - 1;
        while (i<array.size() && j>=0)
        {
            if (target == array[i][j])
                return 1;
            if (target > array[i][j])
                i++;
            else
                j--;
        }
        return 0;
    }
};
```
