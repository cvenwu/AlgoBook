# 数组中出现次数超过一半的数字

## 方法一：摩尔投票法

```go
//使用摩尔投票法
func majorityElement(nums []int) int {
	num, times := nums[0], 1
	for i := 1; i < len(nums); i++ {
		if times == 0 {
			num, times = nums[i], 1
			continue
		}

		if num == nums[i] {
			times++
		} else {
			times--
		}
	}

	return num
}
```

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        //使用摩尔投票法，记录当前次数最多的数字以及出现的次数，如果下一次出现的是另外一个数字，次数就减去1，如果当遍历到下一个数字的时候，且次数为0就将当前数字替换到出现次数最多的，
        int majorElement = nums[0], times = 1;
        for (int i = 1; i < nums.size(); i++)
        {
            //如果当前次数为0
            if (times == 0)
            {
                majorElement = nums[i];
                times = 1;
                continue;
            }
            if (majorElement != nums[i]) //当前遍历到的与目前的主元素不相同，次数-1
                times--;
            else //当前遍历到的与目前的主元素相同，次数+1
                times++;
        }

        return majorElement;
    }
};
```