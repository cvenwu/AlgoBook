# [剑指 Offer 14- II. 剪绳子 II](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/)

## 方法：

参考剑指offer思路，利用贪心的思想，也可以参考LeetCode评论中利用dp


思路：尽可能多的裁减3，但是如果最后剩余1，我们要将和最后1个3搭配，配成2*2

**注意如下两个点：题目中说明了可能溢出，两个地方可能溢出**

1. 求3的幂次方函数中，可能溢出，所以每次乘以3 都要 % 1000000007
2. 最后计算出3的幂次方结果后，范围在1000000007内，但是如果我们最后和4搭配又会溢出，所以最后返回结果的地方还是要% 1000000007

>执行用时：0 ms, 在所有 Go 提交中击败了 100.00% 的用户
		内存消耗：1.9 MB, 在所有 Go 提交中击败了 83.67% 的用户


```go

func pow3(n int) int {
	res := 1
	for i := 0; i < n; i++ {
		//计算3的幂次方，可能溢出，所以要% 1000000007
		res = (res * 3) % 1000000007
	}
	return res
}

func cuttingRope(n int) int {
	if n == 2 {
		return 1
	}else if n == 3 {
		return 2
	}

	timesOf3 := n / 3
	if n - timesOf3 * 3 == 1 {
		timesOf3 -= 1
	}

	timesOf2 := (n - timesOf3 * 3) / 2

	//因为计算得到的3的幂可能在1000000007这个范围内，又因为最后可能乘以4，造成溢出，所以这里也要加上结果
	return pow3(timesOf3) * int(math.Pow(2, float64(timesOf2))) % 1000000007
}
```

