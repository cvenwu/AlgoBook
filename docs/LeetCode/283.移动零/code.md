# [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)





## 方法：

思路：使用一个指针numIndex指向下一次应该存放非0数的位置
		步骤：

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

func main() {

}
```

