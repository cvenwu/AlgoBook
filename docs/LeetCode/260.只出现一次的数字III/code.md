# [260. 只出现一次的数字 III](https://leetcode-cn.com/problems/single-number-iii/)

## 方法一：

思路：

```
1. 首先将所有数异或，得到可以区分两个只出现一次数字的异或结果，
2. 将所有数字分成两组，如果与上面的异或结果异或
3. 将两组中每个组中的所有数字进行异或，得到两个最终结果
4. 返回这两个最终结果
```

> 执行用时：8 ms, 在所有 Go 提交中击败了90.46%的用户
> 		内存消耗：4 MB, 在所有 Go 提交中击败了100.00%的用户

```go

func singleNumber(nums []int) []int {

	xorRet := 0
	for _, num := range nums {
		xorRet ^= num
	}

	//取到异或结果中的最低位
	mask := (xorRet & (-xorRet))

	//根据这一位将数据分成两组
	first, second := 0, 0
	for _, num := range nums {
		if mask & num != 0 {
			first ^= num
		} else {
			second ^= num
		}
	}

	//最后的每组对应的数便是结果
	return []int{first, second}

}

```

## 对方法一的改进：

改进点：对上面代码的小幅改进，不需要使用两个变量进行分别对应我们的结果。同时直接构造mask而不需要重新建变量


> 执行用时：8 ms, 在所有 Go 提交中击败了90.46%的用户
> 		内存消耗：4 MB, 在所有 Go 提交中击败了100.00%的用户

```go

func singleNumber(nums []int) []int {
	xorRet := 0
	for _, num := range nums {
		xorRet ^= num
	}

	//取到异或结果中的最低位
	xorRet = (xorRet & (-xorRet))

	ret := []int{0, 0}
	//根据这一位将数据分成两组
	for _, num := range nums {
		if xorRet & num != 0 {
			ret[0] ^= num
		} else {
			ret[1] ^= num
		}
	}

	//最后的每组对应的数便是结果
	return ret

}
```
