# [977.有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)


## 方法一：自己的方法
![IMG_0381](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/IMG_0381.jpg)

```go
//自己想到的方法：
//执行用时：36 ms, 在所有 Go 提交中击败了69.26%的用户
//内存消耗：7 MB, 在所有 Go 提交中击败了5.38%的用户
func sortedSquares(nums []int) []int {
	//如果传入的数组长度为0直接返回nil
	if len(nums) == 0 {
		return  nil
	}

	//定义要返回的结果
	var ret []int

	//接下来分为3种情况
	if nums[0] >= 0 {	//说明数组中都是正数
		for i := 0; i < len(nums); i++ {
			ret = append(ret, nums[i]*nums[i])
		}
	} else if nums[0] < 0 && nums[len(nums)-1] < 0 {  //说明数组中都是正数
		//倒着遍历
		for i := len(nums)-1; i >= 0; i-- {
			ret = append(ret, nums[i]*nums[i])
		}
	} else {	//说明数组中前半部分是负数，后半部分数非负数
		//采用双指针，分别指向最后一个负数以及第一个非负数，然后谁的平方小，加入结果并且移动谁
		l, r := 0, 0
		for nums[r] < 0 {
			r++
		}
		l = r - 1

		//此时l指向最后一个负数，r指向第一个非负数
		for l != -1 && r != len(nums) {
			if nums[l] * nums[l] > nums[r] * nums[r] {
				ret = append(ret, nums[r] * nums[r])
				r++
				continue
			}

			ret = append(ret, nums[l] * nums[l])
			l--
		}

		//下面两个if只会有一个去执行
		if l != -1 {
			for i := l; i >= 0; i-- {
				ret = append(ret, nums[i]*nums[i])
			}
		}
		if r != len(nums) {
			for i := r; i < len(nums); i++ {
				ret = append(ret, nums[i]*nums[i])
			}
		}
	}
	return ret
}

```

## 【推荐】方法二：其他人的方法

思路：分为两种情况：
1. 第一种全部是整数，
2. 第二种其他情况（包含两种情况，一种是里面有正数和负数，另外一种是里面只有负数，都可以采用双指针从数组的两侧开始遍历，谁的平方大放置谁到最终的结果上并且移动谁）


```go
//执行用时：28 ms, 在所有 Go 提交中击败了98.46%的用户
//内存消耗：6.8 MB, 在所有 Go 提交中击败了58.27%的用户
func sortedSquares(nums []int) []int {
	if len(nums) == 0 {
		return nil
	}

	ret := make([]int, len(nums))

	if nums[0] >= 0 {
		for i := 0; i < len(nums); i++ {
			ret[i] = nums[i]*nums[i]
		}
	} else {
		l, r := 0, len(nums)-1

		//分别从两侧开始遍历，因为两侧的平方一定有某一个是最大的，我们可以将其放到最终位置上
		for i := r; i >= 0; i-- {	//需要循环r次，这次我们放置的位置是从后往前，也就是谁大放谁
			if nums[l] * nums[l] > nums[r] * nums[r] {
				ret[i] = nums[l] * nums[l]
				l++
			} else {
				ret[i] = nums[r] * nums[r]
				r--
			}
		}
	}

	return ret
}

```