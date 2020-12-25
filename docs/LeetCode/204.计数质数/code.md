# [204.计数质数](https://leetcode-cn.com/problems/count-primes/)

## 方法一：暴力
> 这里不再赘述

## 方法二：采用labuladong提供的方法
> 采用去除法，同时如果是质数

> 两个改进点已经注释在了代码中

该方法最终的**时间复杂度：O(NloglogN)**

```go
package main

/**
 * @Author: yirufeng
 * @Date: 2020/12/13 9:39 上午
 * @Desc:
 **/

//执行用时：8 ms, 在所有 Go 提交中击败了95.55%的用户
//内存消耗：4.8 MB, 在所有 Go 提交中击败了92.03%的用户
func countPrimes(n int) int {
	count := 0
	isPrime := make([]bool, n)
	//数组初始化为true
	for i := 0; i < n; i++ {
		isPrime[i] = true
	}

	for i := 2; i*i <= n; i++ { //注意点1：这里我们进行了优化，因为因子对称性，如果一个数i是另外一个数的因数且小于i，那么另外一个因数一定大于i，但是我们只要例如8里面的(2,4)与(4,2)，但是这里我们使用了i*i<=n确保我们只找到一次就可以了
		//如果i是质数
		if isPrime[i] {
			for j := i * i; j < n; j += i { //这里因为i是质数，且前面我们肯定计算过2*i, 3*i,等等直到(i-1)*i，因此这里我们从i*i开始
				isPrime[j] = false
			}
		}
	}

	//数一下质数的个数
	for i := 2; i < n; i++ {
		if isPrime[i] {
			count++
		}
	}
	return count
}

```