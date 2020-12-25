# [201. 数字范围按位与](https://leetcode-cn.com/problems/bitwise-and-of-numbers-range/)

这道题的本质就是求解m和n的最长公共前缀

## 方法一：自己的解法

```
自己的思路：
   求取m和n二进制中最前面的公共位即可，直到某一位不同，就不再继续求了

步骤：
1. 获取最大数n的最高位的1所在的那一位，我们这里用mask表示
2. 之后不断用mask判断m和n是否相同，并累加，如果不等就退出

```

> 时间复杂度：O(N)
> 		空间复杂度：O(1)

```go
func rangeBitwiseAnd(m int, n int) int {
	if m == n {
		return m
	}

	//构造一个mask使得mask的最高位为n的最高位1所在的那1位，从该位开始寻找到某一位，满足一下条件：
	//当向右循环到某1位的时候，左边的所有位m和n都相同，此时直接返回前面的所有位+后面补0即可
	mask := 1
	for mask < n && (mask<<1) <= n {
		mask <<= 1
	}

	ret := 0
	for mask > 0 && mask&m == mask&n {
		ret += mask&m
		mask >>= 1
	}
	return ret
}
```

## 方法二：位移

思路：使用位移算法直接对两个传入的数字偏移

对传入的两个数字进行不断位移，直到两个数相等便是最长公共前缀，直接返回即可

```go
func rangeBitwiseAnd(m int, n int) int {
	shift := 0
	for m>>shift != n>>shift {
		shift += 1
	}
	return m >> shift << shift
}
```

## 【推荐】方法三：Brian Kernighan 算法

算法核心思想：

```
"Brian Kernighan算法可以用于清除二进制数中最右侧的1。Brian Kernighan算法的做法是先将当前数减一,然后在与当前数进行按位与运算。”
每次清除最右边的1，直到两个数获取最长前缀后的n肯定小于等于m,便是最长公共前缀
```

**Brian Kernighan 的算法需要的迭代次数会更少，因为它跳过了两个数字之间的所有零位**

```go
func rangeBitwiseAnd(m int, n int) int {
	for n > m {
		n &= (n-1)
	}
	return n
}
```

