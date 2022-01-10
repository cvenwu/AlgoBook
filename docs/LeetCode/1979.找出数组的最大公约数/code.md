# [1979.辗转相除法](https://leetcode-cn.com/problems/find-greatest-common-divisor-of-array/)


## 方法
!> 2022-01-08 辗转相除法



```c++
class Solution {
public:
    int findGCD(vector<int>& nums) {
        if (nums.empty()) return -1;
        int max = nums[0], min = nums[0];

        for (int i = 1; i < nums.size(); i++)
        {
            if (max < nums[i]) max = nums[i];
            if (min > nums[i]) min = nums[i];
        }


        int temp = 0;
        while(max%min != 0)
        {
            temp = max;
            max = min;
            min = temp % min;
        }

        return min;
    }
};
```


```go
func findGCD(nums []int) int {
    if len(nums) <= 1 {
        return -1
    }

    max, min := nums[0], nums[0]

    //找最小最大
    for i := 0; i < len(nums); i++ {
        if max < nums[i] {
            max = nums[i]
        }
        if min > nums[i] {
            min = nums[i]
        }
    }

    //辗转相除
    for {
        if max % min == 0 {
            break
        }
        max, min = min, max % min
    }
    return min
}
```