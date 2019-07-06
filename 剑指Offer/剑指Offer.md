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
- [5. 替换空格](#5-替换空格)
- [53.1 数字在排序数组中出现的次数（二分查找）](#531-数字在排序数组中出现的次数)
- [53.2 0~n-1中缺失的数字（二分查找）](#532--0-~-n--1-中缺失的数字)
- [53.3 数组中数值和下标相等的元素（二分查找）](#533-数组中数值和下标相等的元素)
-



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

## 4. 二维数组中的查找
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

## 5. 替换空格
>[替换空格](https://www.nowcoder.com/practice/4060ac7e3e404ad1a894ef3e17650423?tpId=13&tqId=11155&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

**题目描述**
```
请实现一个函数，将一个字符串中的空格替换成“%20”。
例如，当字符串为 "We Are Happy". 则经过替换之后的字符串为 "We%20Are%20Happy"。
```

- 示例
```
original string -- "We are happy."
replaced string -- "We%20are%20happy."
```

**思路**
- 第一种是从后向前遍历字符串，遇到空格位置将空格后面字符串后移两位，对应空格位置替换为%20，每次移动的时间复杂度为O(n)，因此总的时间复杂度为O(n^2)。
- 第二种思路是首先统计字符串中空格的数量n，替换后字符串长度为len=len0+2\*n。因为从前往后替换的方法会使后面的字符被覆盖掉，因此需要从后向前替换。每当遇到空格时，就需要将len长度减3，对应空格位置替换为%20，len0长度减1。这种方法的时间复杂度为O(n)。


**代码**
```C++
class Solution {
public:
	void replaceSpace(char *str,int length) {
		// length 为数组总容量
		if(str==nullptr||length<=0)
		{
		    return;
		}
		int count = 0;
		int len = 0;
		while(str[len]!='\0')
		{
		    if(str[len]==' ')
		    {
			++count;
		    }
		    ++len;
		}

		int newlen = len + 2 * count;

		if(newlen>length)
		{
		    return;
		}

		while (len >= 0 && newlen>len)
		{
		    if (str[len] == ' ')
		    {
			str[newlen--] = '0';
			str[newlen--] = '2';
			str[newlen--] = '%';
		    }
		    else
		    {
			str[newlen--] = str[len];
		    }
		    --len;
		}
	}
};
```

## 53.1 数字在排序数组中出现的次数
>[数字在排序数组中出现的次数](https://www.nowcoder.com/practice/70610bf967994b22bb1c26f9ae901fa2tpId=13&tqId=11190&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

**题目描述**
```
统计一个数字在排序数组中出现的次数
```

**思路**
- 可以顺序统计数组中目标数字出现个数，每出现一个总数加1，时间复杂度为O(n)
- 但是在本题中出现了**排序数组**这个额外的条件，因此可以考虑使用最小二乘法，这样时间复杂度为O(logn)
- 我们可以使用最小二乘法查找目标数字第一次出现的位置，当数组中间值等于目标数字，且前一个数字也等于目标数字时，那第一次出现位置肯定在前半段，继续在前半段查找；最后一次出现位置同理，最后出现的总次数时最后一次出现位置减第一次出现位置加1。

**代码**
```C++
class Solution {
public:
    int GetFirstK(vector<int> data,int k,int begin,int end)
    {
        int mid;
        while(begin<=end)
        {
            mid = (begin + end) / 2;
            if(data[mid]==k)
            {
                if(data[mid-1]!=k||mid==0)
                {
                    return mid;
                }
                else
                {
                    end = mid - 1;
                }
            }
            else if(data[mid]>k)
            {
                end = mid - 1;
            }
            else
            {
                begin = mid + 1;
            }
        }
        return -1;
    }
    int GetLastK(vector<int> data,int k,int begin,int end)
    {
        int mid;
        while(begin<=end)
        {
            mid = (begin + end) / 2;
            
            if(data[mid]==k)
            {
                if(data[mid+1]!=k||mid==end)
                {
                    return mid;
                }
                else
                {
                    begin = mid + 1;
                }
            }
            else if(data[mid]>k)
            {
                end = mid - 1;
            }
            else
            {
                begin = mid + 1;
            }
        }
        return -1;
    }
    int GetNumberOfK(vector<int> data ,int k) {
        int len = data.size();
        if(len<=0)
            return 0;
        int count = 0;
        int first = GetFirstK(data,k,0,len-1);
        int last = GetLastK(data,k,0,len-1);
        if(first>-1&&last>-1)
            count = last - first + 1;
        return count;
    }
};
```

## 53.2 0~n-1中缺失的数字

**题目描述**
```
一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0~n-1之内，在范围0~n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。
```

**思路**
- 假设第k个数字不在数组中，那么k之前的每个数字肯定等于其对应的序号，即0对应序号0，1对应序号1，我们只需要找出第一个数字与其序号对应不一致的位置，即为该缺失的数字。

**代码**
```C++
int GetMissingNumber(vector<int> data)
{
    int len = data.size();
    int begin = 0;
    int end = len - 1;
    int mid;
    while(begin<=end)
    {
    	mid = (begin + end) / 2;
	if(data[mid]!=mid)
	{
	    if(data[mid-1]==mid-1||mid==0)
	        return mid;
	    end = mid - 1;
	}
	else
	{
	    start = mid + 1;
	}
	
    }
    return -1;
}
```

## 53.3 数组中数值和下标相等的元素

**题目描述**
```
假设一个单调递增的数组里的每个元素都是整数并且是唯一的。请编程实现一个函数，找出数组中任意一个数值等于其下标的元素。
```

- 示例
```
在数组{-3，-1，1，3，5}中，数字3和它的下标相等。
```

**思路**
- 因为**单调递增**数组，使用二分查找

**代码**
```C++
int GetNumberSameAsIndex(vector<int> data)
{
    int len = data.size();
    int begin = 0;
    int end = len - 1;
    int mid;
    while(begin<=end)
    {
    	mid = (begin + end) / 2;
	if(data[mid]==mid)
	{
	    return mid;
	}
	else if(data[mid]>mid)
	{
	    end = mid - 1;
	}
	else
	{
	    start = mid + 1;
	}
    }
    return -1;
}
```


