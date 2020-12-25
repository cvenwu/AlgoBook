# [1009. 十进制整数的反码](https://leetcode-cn.com/problems/complement-of-base-10-integer/)

思路：类似于476题，唯一的区别在于，这里传入的参数可以为0，因此我们需要额外判断一下即可。

## 方法一：

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：1.9 ms, 在所有 Go 提交中击败了41.18%的用户

```go
func bitwiseComplement(N int) int {
	//如果N为0需要返回1
	if N == 0 {
		return 1
	}

	mask := 1
	for mask <= N {
		mask <<= 1
	}
	return (mask-1)^N
}
```

## 方法二：

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：1.9 ms, 在所有 Go 提交中击败了41.18%的用户

```go
func bitwiseComplement(N int) int {
	//如果N为0需要返回1
	if N == 0 {
		return 1
	}

	mask := ^0
	for mask & N != 0 {
		mask <<= 1
	}
	return ^mask ^ N
}
```

