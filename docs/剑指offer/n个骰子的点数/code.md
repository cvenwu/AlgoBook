# n个骰子的点数


## 方法一：暴力递归
!> 直接提交会超时
```go
//方法一：暴力递归【超时】
func dicesProbability(n int) []float64 {
	ret := make([]float64, 5*n+1)
	dicesProbabilityCore(ret, 0, n, n)
	//遍历ret中的每个值
	sum := math.Pow(6.0, float64(n))
	for i := 0; i < len(ret); i++ {
		ret[i] /= sum
	}
	return ret
}

//第一个表示我们最后的结果，先要统计每个点数对应的出现次数，然后最后除以我们的总可能数
//第二个参数表示当前的点数
//第三个参数表示我们还剩余多少个骰子
//第四个参数表示我们一共有多少个骰子
func dicesProbabilityCore(ret []float64, cur int, num int, n int) {
	if num == 0 {
		ret[cur-n] += 1
		return
	}

	for i := 1; i <= 6; i++ {
		dicesProbabilityCore(ret, cur+i, num-1, n)
	}
}
```

## 方法二：动态规划

!> 思路：![M6EgIZ](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/M6EgIZ.png)

```go
func dicesProbability(n int) []float64 {
	dp := make([][]float64, n+1)
	for i := 0; i <= n; i++ {
		dp[i] = make([]float64, 6*n+1)
	}
	//初始化dp数组的第一行
	for i := 1; i <= 6; i++ {
		dp[1][i]=1
	}

	for i := 2; i <= n; i++ {
		for j := i; j <= 6 * i; j++ {
			for k := 1; k <= 6  && j-k>=0; k++ {
				dp[i][j] += dp[i-1][j-k]
			}
		}
	}

	//遍历数组除以最后的总次数
	sum := math.Pow(6.0, float64(n))
	for j := n; j < 6*n+1; j++ {
		dp[n][j] /= sum
	}
	return dp[n][n:]
}

```


## 【推荐】方法三：对dp进行优化
!> 方法三：对dp进行改进，采用状态压缩
!> 原因：因为发现方法二中的`dp[i][j]`只依赖于`dp[i-1][j-6]+....+dp[i-1][j-1]`
!> 并且更新`dp[i]`的时候注意只能从后向前进行更新，因为一旦前面`dp[i-1]`更改完，`dp[i]`还要依赖于没有修改的`dp[i-1]`
```go
//方法三：对dp进行改进，采用状态压缩
//原因：发现dp[i][j]只依赖于dp[i-1][j-6]+....+dp[i-1][j-1]
//并且更新dp[i]的时候注意只能从后向前进行更新，因为一旦前面dp[i-1]更改完，dp[i]还要依赖于没有修改的dp[i-1]
func dicesProbability(n int) []float64 {
	dp := make([]float64, 6*n+1)
	//初始化dp数组的第一行
	for i := 1; i <= 6; i++ {
		dp[i]=1
	}


	for i := 2; i <= n; i++ {
		for j := 6 * i; j >= i; j-- {
			//先去掉原来的值，避免原来的值的干扰
			dp[j] = 0
			for k := 1; k <= 6; k++ {
				//TODO:注意点1
				//因为当前是投递i个骰子，也就是说前面投了i-1个骰子，最小的和为i-1，但是我们要加上前面的和的时候最小就是i-1不能再小了，
				//因为前面再小的值我们没有赋值为0所以如果加上就会报错
				if j - k < i - 1 {
					break
				}
				dp[j] += dp[j-k]
			}
		}
	}

	//遍历数组除以最后的总次数
	sum := math.Pow(6.0, float64(n))
	for j := n; j < 6*n+1; j++ {
		dp[j] /= sum
	}
	return dp[n:]
}



```