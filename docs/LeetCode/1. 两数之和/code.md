# [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)


## 方法一：暴力
```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int i, j;
        for (i = 0; i < nums.size(); i++)
        {
            //这里其实可以优化一下，j是从i+1开始，因为前面的我们在i为之前的值的时候就已经判断过了
            for (j = i+1; j < nums.size(); j++)
            {
                if ((nums[i] + nums[j]) == target) 
                {
                    return {i, j};
                }
            }
        }
        return {};
    }
};
```

时间复杂度：O(n^2)
空间复杂度：O(1)


## 方法二：使用hmap

```c++
class Solution
{
public:
    vector<int> twoSum(vector<int> &nums, int target)
    {
       unordered_map<int, int> hmap;
       for (int i = 0; i < nums.size(); i++)
       {
           //如果目标值-当前值出现在了hmap中的话，直接返回
           auto it = hmap.find(target-nums[i]);
           if (it != hmap.end())
           {
               //因为返回的是两个组合的索引
               return {it->second, i};
           }
           hmap[nums[i]] = i;
       }
       //否则说明没有找到
       return {};
    }
};
```

时间复杂度：O(n)
空间复杂度：O(n)