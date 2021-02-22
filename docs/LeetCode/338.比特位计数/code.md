# [338. 比特位计数](https://leetcode-cn.com/problems/counting-bits/)

## 方法一：暴力求解

思路：循环统计每个数1的位数即可

## 方法二：dp

思路：利用性质，数字n和数字n&(n-1)的二进制表示差1个1
原因：因为n&(n-1)就是去掉数字n二进制表示中最后一个1
		步骤：

1. base case，dp[0] = 0
2. 状态：dp[i]表示数字i中二进制1的个数
3. 选择：没有选择
4. dp数组：dp[i] = dp[i&(i-1)] + 1

>  执行用时：4 ms, 在所有 Go 提交中击败了91.93%的用户
> 		内存消耗：6.1 ms, 在所有 Go 提交中击败了18.18%的用户


```go

func countBits(num int) []int {
	if num < 0 {
		return []int{}
	}
  
	ret := []int{0}

	for i := 1; i <= num; i++ {
		ret = append(ret, ret[i&(i-1)]+1)
	}

	return ret
}
```

> 2021-02-21更新解法

![k3luLP](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/k3luLP.png)

```go
func countBits(num int) []int {
	ret := make([]int, num+1)
	for i := 1; i <= num; i++ {
		ret[i] = ret[i&(i-1)]+1
	}
	return ret
}

```

