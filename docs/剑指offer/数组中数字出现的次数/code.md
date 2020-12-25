# 数组中数字出现的次数

**思路：分组异或**

## 最初版本：

>  思路：使用原数组，将其分成两部分，前面一部分[0, firstEndPoint]上所有的数字 在 xorRet中最右边为1的二进制位都为1。后面一部分都为的数字 在 xorRet中最右边为1的二进制位都为0
>
> 1. 求出数组中所有数字的异或结果xorRet, 一定不为0，因为有两个只出现1次的数字。
>
> 2. 根据xorRet中的某一个为1的二进制位(这里我们使用最右边为1的二进制位)将该数组中所有数字分成两组，一组在该二进制位上为1，另一组为0
>
> 3. 之后第一组中所有数字进行异或得到的结果便是其中一个只出现一次的数字，另一个以此类推

> //执行用时：24 ms, 在所有 Go 提交中击败了62.56% 的用户
>         //内存消耗：6 MB, 在所有 Go 提交中击败了 100.00% 的用户

```go

func singleNumbers(nums []int) []int {

	//得到两个只出现1次数字的异或结果
	xorRet := 0
	for _, val := range nums {
		xorRet ^= val
	}

	//从右到左找到异或结果不为1的那个二进制位
	//通过这一个二进制位划分成两个数组，
	bitMask := 1
	for i := 0; i < 32; i++ {
		if bitMask & xorRet != 0 {
			break
		}
		bitMask <<= 1
	}
	//上面得到的二进制位和数组中的所有数字与，得到两个结果1或0，可以划分成两个数组
	//前面数组存放结果不为0的
	//后面数组存放结果为0的
	firstEndPoint, secondStartPoint := -1, len(nums)
	for i := 0; i < len(nums); i++ {
		if bitMask & nums[firstEndPoint + 1] != 0 {
			firstEndPoint += 1
		}else {
			nums[firstEndPoint + 1], nums[secondStartPoint - 1] = nums[secondStartPoint - 1], nums[firstEndPoint + 1]
			secondStartPoint -= 1
		}
	}
	//计算结果
	ret1, ret2 := 0, 0
	for i := 0; i <= firstEndPoint; i++ {
		ret1 ^= nums[i]
	}
	for i := firstEndPoint + 1; i < len(nums); i++ {
		ret2 ^= nums[i]
	}
	fmt.Println(ret1, ret2)
	//得到结果
	ret := make([]int, 2)
	ret[0], ret[1] = ret1, ret2
	//返回结果
	return ret
}
```

该方法缺点：

1. 遍历了3次数组
2. 同时对元素组进行了修改，需要提前问面试官是否可以修改原数组

## 改进版本

> //执行用时：20 ms, 在所有 Go 提交中击败了89.35% 的用户
>         //内存消耗：6 MB, 在所有 Go 提交中击败了 100.00% 的用户

```go
//上面方法的改进，只用遍历两次数组即可
func singleNumbers(nums []int) []int {

	//得到两个只出现1次数字的异或结果
	xorRet := 0
	for _, val := range nums {
		xorRet ^= val
	}

	//从右到左找到异或结果不为1的那个二进制位
	//通过这一个二进制位划分成两个数组，
	bitMask := 1
	for i := 0; i < 32; i++ {
		if bitMask & xorRet != 0 {
			break
		}
		bitMask <<= 1
	}

	//上面得到的二进制位和数组中的所有数字与，得到两个结果1或0，可以划分成两个数组
	ret1, ret2 := 0, 0
	for i := 0; i < len(nums); i++ {
		if bitMask & nums[i] != 0 {
			ret1 ^= nums[i]
		}else {
			ret2 ^= nums[i]
		}
	}
	//得到结果
	ret := make([]int, 2)
	ret[0], ret[1] = ret1, ret2

	//返回结果
	return ret
}
```

改进：

1. 只用遍历两次数组



## 进阶

> //执行用时：20 ms, 在所有 Go 提交中击败了89.35% 的用户
>         //内存消耗：6 MB, 在所有 Go 提交中击败了 100.00% 的用户

```go

func singleNumbers(nums []int) []int {

	//得到两个只出现1次数字的异或结果
	xorRet := 0
	for _, val := range nums {
		xorRet ^= val
	}

	//得到最右边的为1的二进制
	xorRet = xorRet & (-xorRet)  //相当于a = a &（~a+1
	fmt.Println(xorRet)
	//上面得到的二进制位和数组中的所有数字与，得到两个结果1或0，可以划分成两个数组
	ret1, ret2 := 0, 0
	for i := 0; i < len(nums); i++ {
		if xorRet & nums[i] != 0 {
			ret1 ^= nums[i]
		}else {
			ret2 ^= nums[i]
		}
	}
	fmt.Println(ret1, ret2)
	//返回结果
	return []int{ret1, ret2}
}
```

改进：

1.  通过 `x = x & -x*` 找到最右边的二进制位1
2. 返回切片的时候 不用直接make， 可以直接使用 return []int{ret1, ret2} 相当于返回一个长度为2的切片 `*return* []int{ret1, ret2}`



## 进阶-2

> //执行用时：20 ms, 在所有 Go 提交中击败了89.35% 的用户
>         //内存消耗：6 MB, 在所有 Go 提交中击败了 100.00% 的用户

```go

func singleNumbers(nums []int) []int {

	//得到两个只出现1次数字的异或结果
	xorRet := 0
	for _, val := range nums {
		xorRet ^= val
	}

	//得到最右边的为1的二进制
	xorRet = xorRet & (-xorRet)  //相当于a = a &（~a+1

	//上面得到的二进制位和数组中的所有数字与，得到两个结果1或0，可以划分成两个数组
	ret := make([]int, 2)
	for i := 0; i < len(nums); i++ {
		if xorRet & nums[i] != 0 {
			ret[0] ^= nums[i]
		}else {
			ret[1] ^= nums[i]
		}
	}

	//返回结果
	return ret
}
```

改进：**直接使用切片存储而不需要两个结果变量**

