# [剑指 Offer 46. 把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

## 方法：dp


> 因为涉及到了重复计算，所以我们采用dp来进行计算

假设字符串为numString
步骤：

1. base case ：`dp[0]` = 1, `dp[1]` = 1 或 2(条件是倒数2个字符为10-25之间)
2. 状态：`dp[i]` 表示前i+1位翻译成字符串的结果数
3. 转移方程：
          1. `dp[i] = dp[i-1]`    如果`numString[i-1:i+1]`不在10-25之间
          2. `dp[i-1] + dp[i-2]`  如果`numString[i-1:i+1]`在10-25之间
4. 我们要的结果：dp数组的最后一个

**优化【状态压缩】：发现使用3个滚动变量就可以进行状态优化来完成我们整个功能**

> 执行用时：0 ms, 在所有 Go 提交中击败了 100.00% 的用户
		内存消耗：1.9 MB, 在所有 Go 提交中击败了 57.14% 的用户

```go
func translateNum(num int) int {
	if num < 0 {
		return 0
	} else if num >= 0 && num <= 9 {
		return 1
	}

	numString := strconv.Itoa(num)


	//p, q 代表dp[i-1], dp[i-2]
	p, q, r := 1, 1, 0
	if numString[0:2] >= "10" && numString <= "25" {
		p++
	}

	//注意：如果输入的只有一个两位数
	//这里将p赋值为r是为了避免输入一个两位数，到时候不进入下面的循环，直接返回r
	r = p

	for i := 2; i < len(numString); i++ {
		//如果前面两位是可以转换成字符串的
		if numString[i-1:i+1] >= "10" && numString[i-1:i+1] <= "25" {
			r = p + q
		} else {
			r = p
		}
		q, p = p, r
	}
	return r
}
```



