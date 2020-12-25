# [905. 按奇偶排序数组](https://leetcode-cn.com/problems/sort-array-by-parity/)

## 方法一：自己的解法

思路：对撞指针

前面指针存放偶数元素，后面指针指向奇数元素

>  执行用时：12 ms, 在所有 Go 提交中击败了54.79%的用户
> 		内存消耗：4.7 ms, 在所有 Go 提交中击败了47.73%的用户

```go
func sortArrayByParity(A []int) []int {
	if len(A) <= 1 {
		return A
	}

	e, o := 0, len(A)-1
	for e < o {
		//注意判断e越界
		for e < len(A) && A[e]&1 == 0 {
			e++
		}
		//注意判断o越界
		for o >= 0 && A[o]&1 == 1 {
			o--
		}

		if e >= o {
			break
		}

		if e < len(A) && o >= 0 {
			A[o], A[e] = A[e], A[o]
			o--
			e++
		}
	}

	return A
}
```

## 【推荐】方法二：

改进：不用管边界，因为上面用到了for循环，需要注意循环超过了边界


> 执行用时：12 ms, 在所有 Go 提交中击败了54.79%的用户
> 		内存消耗：4.7 ms, 在所有 Go 提交中击败了56.82%的用户


```go
func sortArrayByParity(A []int) []int {
	l, r := 0, len(A)-1
	for l < r {
		if A[l]&1 == 0 {
			l++
		} else if A[r]&1 != 0 {
			r--
		} else {
			A[l], A[r] = A[r], A[l]
			l++
			r--
		}
	}
	return A
}
```

