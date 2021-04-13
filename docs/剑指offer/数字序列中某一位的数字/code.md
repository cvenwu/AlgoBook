# [剑指 Offer 44. 数字序列中某一位的数字](https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/)

## 方法一：
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



## 【推荐】方法二：改进
规律题：
```
10 11 12 ... 98 99              --> 90   个数字每个数字占两位
100 101 102 ... 998 999         --> 900  个数字每个数字占三位
1000 1001 1002 ... 9998 9999    --> 9000 个数字每个数字占四位

第一行共有 10*9*2 个数
第二行公有 10*10*9*3 个数
第三行公有 10*10*10*9*4 个数

好了规律出来了:
假设初始标记count = 1
每行总数字为：10**count * 9 * (count+1)

另外关于整除后的余数：
如果没有余数，那结果就是基数（10**count） + 商 - 1，然后获取这个数的最后一个数字即可
如果有余数，那结果就是基数（10**count） + 商，获取到当前的数字，然后 余数-1 找到对应index返回即可
```
!> [参考](https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/solution/lai-kan-kan-fen-xi-ni-hui-jue-de-zhe-dao-3o0e/)


```go
func findNthDigit(n int) int {
	//如果当前数字小于等于9直接返回
	if n <= 9 {
		return n
	}
	//n表示当前还有多少位到达我们想要的那个数字
	//count表示当前是几位数,temp表示当前是几位数的开始（比如3位数的话就是100，4位数就是1000）
	count, temp := 1, 1
	for n > 9*temp*count {
		//说明不在当前的count位数表示的范围内，因此减去这个范围内的数的个数
		n -= 9 * temp * count
		temp, count = temp*10, count+1
	}
	//因为我们1位数的时候是0-9算了9次，但是实际上是10个数，所以要多减去1
	n--
	//此时得到的n在count位数这个范围内，找到对应的数字即可
	//比如传入的n=11，那么这里i,j分别是0，1，并且temp是两位数10
	i, j := n/count, n%count
	return int([]byte(strconv.Itoa(temp + i))[j]-'0')
}
```