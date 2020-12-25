# [27. 移除元素](https://leetcode-cn.com/problems/remove-element/)



## 方法一：

思路：使用两个索引，其中1个从前往后指向我们要删除的值，另外一个从后往前，指向非删除值的元素。之后交换。

注意点：两个索引用start以及end代替，由于初始情况下，我们不知道第一个元素是否为要删除的元素，所以我们**初始状态让start=-1，避免数组里面全部都是要删除的值的元素。** **也就是说，我们让start的意义：指向的元素我们可以理解为已经没有要删除元素值的数组的最后一个位置。因而最后返回的新数组的长度应该为start+1**



```go
//执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
//内存消耗：2.1 MB, 在所有 Go 提交中击败了85.68%的用户
//这道题自己没做出来就卡在这里，关键核心special case 如何处理输入的一个数组全部都是我们要删除元素的情况
//✅：自己可以使用一个指针，从-1开始，如果没有找到就指向-1
//这次改变了start的意义：已经确定过没有我们要找的元素的数组对应的最后一个位置的下标，
func removeElement(nums []int, val int) int {
	//包含两种情况：一种是数组本来就为空，另一种是数组只有一个我们要删除的元素
	if len(nums) == 0 || len(nums) == 1 && nums[0] == val {
		return 0
	}

	//如果数组长度为1并且不是我们要删除的元素直接返回1
	if len(nums) == 1 && nums[0] != val {
		return 1
	}

	//start指向第一个值为val的元素，end指向最后一个值不为val的元素
	start, end := -1, len(nums)-1

	for start < end {
		for start < end && nums[start+1] != val {
			start++
		}

		for start < end && nums[end] == val {
			end--
		}

		//如果循环移动之后位置依然合理
		if start < end {
			nums[start+1], nums[end] = nums[end], nums[start+1]
			start++
			end--
		}
	}



	return start + 1
}


```



**另外一种写法：**

```go
//第二种写法
//另外一种写法：上面的for循环可以改写成下面这样
//执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
//内存消耗：2.1 MB, 在所有 Go 提交中击败了49.68%的用户
func removeElement(nums []int, val int) int {
	//包含两种情况：一种是数组本来就为空，另一种是数组只有一个我们要删除的元素
	if len(nums) == 0 || len(nums) == 1 && nums[0] == val {
		return 0
	}

	//如果数组长度为1并且不是我们要删除的元素直接返回1
	if len(nums) == 1 && nums[0] != val {
		return 1
	}

	//start指向第一个值为val的元素，end指向最后一个值不为val的元素
	start, end := -1, len(nums)-1

	for start < end {
		if nums[start+1] != val {
			start++
			continue
		}

		if nums[end] == val {
			end--
			continue
		}

		//如果循环移动之后位置依然合理
		nums[start+1], nums[end] = nums[end], nums[start+1]
		start++
		end--
	}

	return start + 1
}
```







## ~~错误方法~~

> 由于直接让start从第1个元素开始，并不知道第1个元素是否该删除。

```go
//自己第一时间想出来的方法：
//采用两个指针，一个指向第一个值为val的元素，以及最后一个值不是val的元素，然后交换
//❌：因为可能输入的数组全部都是我们要移除的数
func removeElement(nums []int, val int) int {
	//包含两种情况：一种是数组本来就为空，另一种是数组只有一个我们要删除的元素
	if len(nums) == 0 || len(nums) == 1 && nums[0] == val {
		return 0
	}

	//如果数组长度为1并且不是我们要删除的元素直接返回1
	if len(nums) == 1 && nums[0] != val {
		return 1
	}
	//start指向第一个值为val的元素，end指向最后一个值不为val的元素
	start, end := 0, len(nums)-1

	for start < end {
		for start < end && nums[start] != val {
			start++
		}

		for start < end && nums[end] == val {
			end--
		}

		//如果循环移动之后位置依然合理
		if start < end {
			nums[start], nums[end] = nums[end], nums[start]
			start++
			end--
		}
	}

	//另外一种写法：上面的for循环可以改写成下面这样
	//for start < end {
	//	if nums[start] != val {
	//		start++
	//		continue
	//	}
	//
	//	if nums[end] == val {
	//		end--
	//		continue
	//	}
	//
	//	//如果循环移动之后位置依然合理
	//	nums[start], nums[end] = nums[end], nums[start]
	//	start++
	//	end--
	//}

	return start + 1
}
```



