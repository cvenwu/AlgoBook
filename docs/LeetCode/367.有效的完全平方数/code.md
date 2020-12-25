# [367. 有效的完全平方数](https://leetcode-cn.com/problems/valid-perfect-square/)

## 方法一：二分

注意：mid * mid容易溢出

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2 MB, 在所有 Go 提交中击败了50.00%的用户

```go
func isPerfectSquare(num int) bool {
	left, right := 1, num
	for left <= right {
		mid := left + (right-left)>>1
		if mid*mid < num {
			left = mid + 1
		} else if mid*mid > num {
			right = mid - 1
		} else if mid*mid == num {
			return true
		}
	}

	return false
}
```





## 方法二：对上面代码进行改进

改进：避免溢出(不适用mid * mid)，同时增加了num 为1的情况

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.1 MB, 在所有 Go 提交中击败了17.31%的用户

```go
func isPerfectSquare(num int) bool {
	if num == 1 {
		return true
	}

	left, right := 1, num
	for left <= right {
		mid := left + (right-left)>>1
		if mid < num/mid { //这里为了避免溢出
			left = mid + 1
		} else if mid > num/mid { //这里为了避免溢出
			right = mid - 1
		} else if (num%mid == 0) && (num/mid == mid) { //这里为了避免溢出
			return true
		} else if (num%mid != 0) && (num/mid == mid) {
			left = mid + 1
		}
	}

	return false
}
```

## 方法三：牛顿迭代法

[参考](https://leetcode-cn.com/problems/valid-perfect-square/solution/you-xiao-de-wan-quan-ping-fang-shu-by-leetcode/)