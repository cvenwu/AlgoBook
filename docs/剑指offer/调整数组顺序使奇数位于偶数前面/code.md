# 调整数组顺序使奇数位于偶数前面

## 方法一：
!> 思路：采用双指针，左指针最开始指向数组最左边元素，右指针指向最右边元素。
条件1：当左指针指向的数为奇数，则左指针右移
条件2：当右指针指向的数为偶数，则右指针左移

!> 注意：两个数交换必须在条件1和条件2之前，因为一旦移动，那么我们交换有可能左指针已经在右指针右边了。(如果在每一个分支下面写一个continue就可以避免这个要求)

**Tips：Golang中的代码块1一定要在代码块2和代码块3前面，否则对于下面输入会报错** `[11,9,3,7,16,4,2,0]`
**错误将会输出 [11,9,3,16,7,4,2,0]**
**预期结果是[11,9,3,7,16,4,2,0]**

```go
func exchange(nums []int) []int {
	//如果给了一个空数组
	//这个if成立的时候for循环也不会执行，因此这个if可以不写
	if len(nums) == 0 {
		return []int{}
	}

	//odd用来做奇数，even用来做偶数
	odd, even := 0, len(nums) - 1
	for odd < even {
		//如果odd为偶数，even指向奇数则交换
		//代码块1
		if ((nums[odd] & 1) == 0) && ((nums[even] & 1) != 0) {
			nums[odd], nums[even] = nums[even], nums[odd]
			//一定要记得改变odd以及even
			odd, even = odd + 1, even - 1
		}

		//代码块2
		//如果odd为奇数则后移
		if (nums[odd] & 1) != 0 {
			odd += 1
		}

		//代码块3
		//如果even为偶数则前移
		if (nums[even] & 1) == 0 {
			even -= 1
		}
	}
	return nums
}
```

```c++
//下面的解法并不能保证相对顺序依然不会改变
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        //如果数组为空直接返回
        if (nums.size() <= 1) return nums;

        //left左边是已经放置好的奇数，right右边是已经放置好的偶数
        int left = 0, right = nums.size()-1;
        while(left < right)
        {
            //如果left是奇数，直接右移
            if (nums[left] & 1) 
            {
                left++;
                continue;
            }
            //如果right是偶数，直接左移
            if ((nums[right] & 1) == 0) 
            {
                right--;
                continue;
            }
            //如果left是偶数，right是奇数就交换
            if ((nums[left] & 1) == 0 && (nums[right] & 1)) 
            {
                //直接交换两个数字
                int temp = nums[left];
                nums[left] = nums[right];
                nums[right] = temp;
                //交换之后
                left++;
                right--;
            }
        }

        //最后返回结果
        return nums;
    }
};

```



## 【推荐】方法二：

!> 思路：使用两个指针。
1. 第一个指针的含义表示我们奇数数组的最后一个元素的索引
2. 第二个指针指向我们非奇数数组的第一个元素


```go
func exchange(nums []int) []int {
	i, j := -1, 0
	for j < len(nums) {
		if nums[j]&1 == 1 {
			nums[j], nums[i+1] = nums[i+1], nums[j]
			i++
		}
		j++
	}
	return nums
}

```