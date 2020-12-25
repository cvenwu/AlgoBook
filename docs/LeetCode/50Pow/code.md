# [50. Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)

## 方法一：快速幂+递归

时间复杂度：O(logN)
		空间复杂度：O(logN)

```go
func myPow(x float64, n int) float64 {
	//如果底数为0
	if x == 0 {
		return 0
	}
	//如果指数为0
	if n == 0 {
		return 1
	}
	absN := n
	//如果是负指数
	if absN < 0 {
		absN = -1 * n
	}
	//分治
	ret := myPow(x, absN >> 1)
	ret *= ret
	if absN & 1 == 1 {
		ret *= x
	}

	if n < 0 {
		return 1 / ret
	}

	return ret
}
```

## 方法二：快速幂+迭代

时间复杂度：O(logN)
		空间复杂度：O(1)

改进：将递归所需的空间复杂度降为O(1)

