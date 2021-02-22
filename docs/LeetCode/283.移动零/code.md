# [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

## 方法一：
> 思路：双指针，前面一个指针指向第一个0，后面一个指针指向第一个0之后的第一个非0元素，不断交换，直到不可以交换为止

```go
func moveZeroes(nums []int)  {
	i, j := 0, 0
	for {
		for i < len(nums) && nums[i] != 0 {
			i++
		}
		j = i + 1
		for j < len(nums) && nums[j] == 0 {
			j++
		}

		if i >= len(nums) || j >= len(nums) {
			return
		}

		nums[i], nums[j] = nums[j], nums[i]
	}
}

```

## 方法二：参考LeetCode题解

> 双指针，只是两个指针的含义不同，
> 第一个指针指向已经满足题意的数组的最后一个元素(即该数组中没有0)，
> 第二个指针指向后面的不满足题意中的第一个非0元素，之后与第一个指针下一个位置进行交换，然后第一个指针+1，直到第2个指针超过了数组的长度
```go
//另外一种方法，参考LeetCode题解
//【推荐】思路以及代码都很简单

func moveZeroes(nums []int)  {
	//初始化两个指针
	l, r := -1, 0
	for r < len(nums) {
		//交换到l+1位置
		if nums[r] != 0 {
			nums[l+1], nums[r] = nums[r], nums[l+1]
			l++
		}
		r++
	}
}

```

## 【推荐】【最优解】方法三：

> 思路：使用一个指针numIndex指向下一次应该存放非0数的位置

**步骤：**
1. 遍历每个元素，如果非0，直接存到指针numIndex+1指向的位置，之后指针后移一个单位
2. 最后所有非0元素存放完之后，直接将剩下的元素从numIndex+1开始全部存0
3. 最后返回

>  执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：3.7 ms, 在所有 Go 提交中击败了99.91%的用户

```go
package main

/**
 * @Author: yirufeng
 * @Email: yirufeng@foxmail.com
 * @Date: 2020/9/28 5:03 下午
 * @Desc: 283. 移动零
 */


/*
思路：使用一个指针numIndex指向下一次应该存放非0数的位置
步骤：
1. 遍历每个元素，如果非0，直接存到指针numIndex+1指向的位置，之后指针后移一个单位
2. 最后所有非0元素存放完之后，直接将剩下的元素从numIndex+1开始全部存0
3. 最后返回

执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
内存消耗：3.7 ms, 在所有 Go 提交中击败了99.91%的用户
 */
func moveZeroes(nums []int)  {
	numIndex := -1
	for i := 0; i < len(nums); i++ {
		if nums[i] != 0 {
			nums[numIndex+1] = nums[i]
			numIndex++
		}
	}

	//此时将所有非0元素放到左边之后，numIndex+1此时指向的是存放第1个0的位置
	for i := numIndex+1; i < len(nums); i++ {
		nums[i] = 0
	}
}
```

