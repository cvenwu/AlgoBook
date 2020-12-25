# [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/) 

具体思路[看自己的博客文章](http://www.sivan.tech/2020/12/08/%E5%AF%BB%E6%89%BE%E6%97%8B%E8%BD%AC%E6%8E%92%E5%BA%8F%E6%95%B0%E7%BB%84%E4%B8%AD%E7%9A%84%E5%80%BC/)

## 方法一：暴力

## 方法二：二分

```go
/** ✅
 * @Author: yirufeng
 * @Date: 2020/12/3 7:52 下午
 * @Desc:
执行用时：4 ms, 在所有 Go 提交中击败了78.30%的用户
内存消耗：2.5 MB, 在所有 Go 提交中击败了96.57%的用户
 **/
func search(nums []int, target int) int {
	if len(nums) == 0 {
		return 0
	}

	l, r := 0, len(nums)-1
	for l <= r {
		mid := l + (r-l)>>1
		if nums[mid] == target {
			return mid
		}

		if nums[mid] < nums[r] { //说明[mid, r]上是递增的

			if target < nums[mid] {
				r = mid - 1
			} else if target > nums[mid] {
				if target <= nums[r] {
					l = mid + 1
				} else if target > nums[r] {
					r = mid - 1
				}
			}

		} else if nums[mid] > nums[r] { //说明此时[l, mid]上是递增的
			if target > nums[mid] {
				l = mid + 1
			} else if target < nums[mid] {
				if target >= nums[l] {
					r = mid - 1
				} else if target < nums[l] {
					l = mid + 1
				}
			}

		} else if nums[mid] == nums[r] { //说明此时l,r以及mid指向同一个元素
			//此时判断该元素是否为目标值，
			//因为在前面我们执行了nums[mid]==target所以这里肯定不是等于目标值
			//所以返回-1
			return -1
		}
	}

	//一定不会执行到这里
	return -1
}
```

## 优化后的代码：

```go
//对上面的代码进行优化
//执行用时：4 ms, 在所有 Go 提交中击败了78.30%的用户
//内存消耗：2.5 MB, 在所有 Go 提交中击败了96.57%的用户
func search(nums []int, target int) int {
	if len(nums) == 0 {
		return 0
	}

	l, r := 0, len(nums)-1
	for l <= r {
		mid := l + (r-l)>>1
		if nums[mid] == target {
			return mid
		}

		if nums[mid] < nums[r] { //说明[mid, r]上是递增的

			/*if target < nums[mid] {
				r = mid - 1
			} else if target > nums[mid] {
				if target <= nums[r] {
					l = mid + 1
				} else if target > nums[r] {
					r = mid - 1
				}
			}*/

			//上面注释代码改成这样
			if target > nums[mid] && target <= nums[r] {
				l = mid + 1
			} else {
				r = mid - 1
			}
		} else if nums[mid] > nums[r] { //说明此时[l, mid]上是递增的
			/*if target > nums[mid] {
				l = mid + 1
			} else if target < nums[mid] {
				if target >= nums[l] {
					r = mid - 1
				} else if target < nums[l] {
					l = mid + 1
				}
			}*/

			//上面注释代码改成这样
			if target < nums[mid] && target >= nums[l] {
				r = mid - 1
			} else {
				l = mid + 1
			}

		} else if nums[mid] == nums[r] { //说明此时l,r以及mid指向同一个元素
			//此时判断该元素是否为目标值，
			//因为在前面我们执行了nums[mid]==target所以这里肯定不是等于目标值
			//所以返回-1
			return -1
		}
	}

	//一定不会执行到这里
	return -1
}

```

