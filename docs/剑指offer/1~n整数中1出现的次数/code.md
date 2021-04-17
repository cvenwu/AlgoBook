# [剑指 Offer 43. 1～n整数中1出现的次数](https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/)

## 方法一：参考左老师的思路

步骤：

1. 首先求我们传入数字n的位数：假设为bitsize
2. 构造该数字的最高位1其他位为0的数字：例如传入21345我们构造出10000。假设构造出来的数字为tmp1
3. 求最高位上1出现的次数：
   1. 如果最高位为1，那么最高位1出现的次数为：n % tmp1 + 1
   2. 如果最高位不为1，那么最高位1出现的次数为：tmp1
4. 除了最高位上其他位上1总共出现的次数：使用公式进行计算，`最高位的数字 * (bitsize - 1) * (10 ^ (bitsize-2))` 因为除去最高位之后，剩下的每一位都有可能被固定为1（除去最高位以及固定的某位为1，剩下的只是bitsize - 1），之后剩余的位都会在0-9之间变化（也就是有10 ^ (bitsize - 1)次方种可能）。因此我们有这个公式，
5. 此时我们计算得到的是`num % tmp1 + 1 ~ num这个区间位1出现的次数`，因此还需要递归判断剩余的数字
**注意：不要忘记递归条件，以及特殊输入(0)**


> 执行用时：0 ms, 在所有 Go 提交中击败了 100.00% 的用户
> 		内存消耗：1.9 MB, 在所有 Go 提交中击败了 47.50% 的用户

```go
func countDigitOne(n int) int {

	if n <= 0 {
		return 0
	}

	//注意：递归结束条件一定要记得加上
	if n <= 9 {
		return 1
	}

	//统计1出现的次数
	count := 0

	//1. 求得传入数字的位数
	bitsize := getBitSizeOfNum(n)

	//2. 构造一个数字使得该数字在我们传入的数字的最高位上为1其他位为0
	temp := 1
	for i := 0; i < bitsize-1; i++ {
		temp *= 10
	}

	//3. 求最高位上1出现的次数
	//如果最高位为1
	if n/temp == 1 {
		count += (n % temp) + 1
	} else { //说明最高位不为1
		count += temp
	}

	//4. 求其他位上1出现的次数
	count += (temp / 10) * (n / temp) * (bitsize - 1)

	//count我们求的是从n % temp + 1到n中1出现的次数
	//因此我们还需要递归求1-n%temp中1出现的次数
	return count + countDigitOne(n%temp)

}

func getBitSizeOfNum(n int) int {
	count := 0
	for n != 0 {
		count += 1
		n /= 10
	}
	return count
}
```

