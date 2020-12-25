# [66. 加一](https://leetcode-cn.com/problems/plus-one/)

## 方法一：【自己的解法】

思路：从数组的最后一位依次从最后面进行遍历，然后每次加一并进行计算

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.1 ms, 在所有 Go 提交中击败了5.10%的用户


```go
func plusOne(digits []int) []int {
	if len(digits) <= 0 {
		return nil
	}

	ret := []int{(1 + digits[len(digits)-1]) % 10}
	carry := (1 + digits[len(digits)-1]) / 10
	//遍历传入的数组中的每一个元素，数组的最后一个元素上要加一
	for i := len(digits) - 2; i >= 0; i-- {
		ret = append(ret, (carry+digits[i])%10)
		carry = (carry + digits[i]) / 10
	}

	//说明到最后数组遍历完了，但是还是有进位
	if carry != 0 {
		ret = append(ret, 1)
	}

	//反转ret
	for i := 0; i < len(ret)>>1; i++ {
		ret[i], ret[len(ret)-1-i] = ret[len(ret)-1-i], ret[i]
	}
	//将我们反转后的结果进行返回
	return ret
}
```

## 【推荐】方法二：【参考提交用时100%】


> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2 ms, 在所有 Go 提交中击败了41.43%的用户


```go
func plusOne(digits []int) []int {
	for i := len(digits)-1; i >= 0; i-- {
		//如果当前位不是9，加一后直接返回
		if digits[i] != 9 {
			digits[i] += 1
			return digits
		}
		//如果当前位是9，说明该位会变成0
		digits[i] = 0
	}

	//走到最后，说明还有1个1要加到最前面
	digits[0] = 1

	//最后还要加上0
	return append(digits, 0)
}

```

