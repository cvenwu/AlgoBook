# [剑指 Offer 44. 数字序列中某一位的数字](https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/)

## 方法：
思路：
1. 首先要找的位数在几位数之间，找到在几位数之后，从该数字开始，然后我们可以直接找到我们要的那个数，但是在第几位还是要根据取模来进行判断
2. 例子：假设我们要找的是第194位数字，
   过程：
   1. 位数为1时，只有10个位，所以我们要让位数+1，从后面找184位
   2. 位数为2时，只有180个位，所以我们要让位数+1，从后面找4个位
   3. 位数为3时，可以找到，找的数从3位数的起始100开始，我们要从后面找4位，直接定位到我们要的那个数字100 + 4 / 3
     然后从101开始找我们要的那个位，该位在 101中的第 4 % 3 个位置上，也就是第1个位置(我们位置是从0开始的)，所以0就是我们要找的

> 执行用时：0 ms, 在所有 Go 提交中击败了 100.00% 的用户
			内存消耗：1.9 MB, 在所有 Go 提交中击败了 53.33% 的用户

```go
func countDigit(digit int) int {
	if digit == 1 {
		return 10
	}

	start := 1
	for i := 0; i < digit-1; i++ {
		start *= 10
	}

	//统计位数记得最后乘以位的个数
	return (start*10 - start) *digit
}

//找到位数为digit位的第一个数字
func startNum(digit int) int {
	if digit == 1 {
		return 0
	}

	start := 1
	for i := 0; i < digit-1; i++ {
		start *= 10
	}

	return start
}

func searchIndexDigit(index, digit int) int {
	num := startNum(digit)

	//找到我们要的那个数字
	num += index / digit
	//从右边数是第几位数字
	fromRight := digit - (index % digit)

	//最后我们要返回的结果，也就是第n位序列上的数
	ret := 0
	for i := 0; i < fromRight; i++ {
		ret = num % 10
		num = num / 10

	}

	return ret
}

func findNthDigit(n int) int {
	digit := 1
	for {
		count := countDigit(digit)
		if count < n+1{
			n -= count
			digit += 1
		} else {
			//执行查找函数
			return searchIndexDigit(n, digit)
		}
	}
}

```

