# [81. 搜索旋转排序数组 II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)

具体思路[看自己的博客文章](http://www.sivan.tech/2020/12/08/%E5%AF%BB%E6%89%BE%E6%97%8B%E8%BD%AC%E6%8E%92%E5%BA%8F%E6%95%B0%E7%BB%84%E4%B8%AD%E7%9A%84%E5%80%BC/)



## 方法一：暴力

## 方法二：二分

```go
//不同于33题就在于nums[mid]==nums[r]的情况，此时因为数组中有重复元素，不同于33题，并不能判断l, mid, r都指向同一个元素，所以只能不断缩减区间
//执行用时：4 ms, 在所有 Go 提交中击败了90.74%的用户
//内存消耗：3.2 MB， 在所有 Go 提交中击败了95.60%的用户
func search(nums []int, target int) bool {
	if len(nums) == 0 {
		return false
	}

	l, r := 0, len(nums)-1
	for l <= r {
		mid := l + (r-l)>>1
		if target == nums[mid] {
			return true
		}

		if nums[mid] > nums[r] { //说明此时[l, mid]是单调递增的
			if target > nums[mid] {
				l = mid + 1
			} else if target < nums[mid] {
				if target >= nums[l] {
					r = mid - 1
				} else if target < nums[l] {
					l = mid + 1
				}
			}
		} else if nums[mid] < nums[r] {
			if target < nums[mid] {
				r = mid - 1
			} else if target > nums[mid] {
				if target <= nums[r] {
					l = mid + 1
				} else if target > nums[r] {
					r = mid - 1
				}
			}

		} else if nums[mid] == nums[r] { //关键点：不同于33题的
			//因为数组中有重复元素，我们不能确定l, mid, r是指向同一个元素的
			//因此让r不断缩减
			r--
		}
	}

	//最后一定不会执行到这里
	return false

}
```



## 优化后的代码：

```go

//对上面的代码进行优化
//执行用时：4 ms, 在所有 Go 提交中击败了90.74%的用户
//内存消耗：3.2 MB， 在所有 Go 提交中击败了95.60%的用户
func search(nums []int, target int) bool {
	if len(nums) == 0 {
		return false
	}

	l, r := 0, len(nums)-1
	for l <= r {
		mid := l + (r-l)>>1
		if target == nums[mid] {
			return true
		}

		if nums[mid] > nums[r] { //说明此时[l, mid]是单调递增的
			//if target > nums[mid] {
			//	l = mid + 1
			//} else if target < nums[mid] {
			//	if target >= nums[l] {
			//		r = mid - 1
			//	} else if target < nums[l] {
			//		l = mid + 1
			//	}
			//}
			if target < nums[mid] && target >= nums[l] {
				r = mid - 1
			} else {
				l = mid + 1
			}
		} else if nums[mid] < nums[r] {
			//if target < nums[mid] {
			//	r = mid - 1
			//} else if target > nums[mid] {
			//	if target <= nums[r] {
			//		l = mid + 1
			//	} else if target > nums[r] {
			//		r = mid - 1
			//	}
			//}

			if target > nums[mid] && target <= nums[r] {
				l = mid + 1
			} else {
				r = mid - 1
			}

		} else if nums[mid] == nums[r] { //关键点：不同于33题的
			//因为数组中有重复元素，我们不能确定l, mid, r是指向同一个元素的
			//因此让r不断缩减
			r--
		}
	}

	//最后一定不会执行到这里
	return false

}

```

