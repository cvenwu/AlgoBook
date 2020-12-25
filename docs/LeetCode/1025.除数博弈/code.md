# [1025. 除数博弈](https://leetcode-cn.com/problems/divisor-game/)



## 方法一：

**核心点：谁最后拿到1谁必输**

因此围绕上面的核心点，有如下两种情况：

Case 1: 我们拿到的是一个奇数，我们只能选1个奇数，之后对方将会拿到一个偶数，对方肯定很聪明，也会一直选1，逼我们拿到奇数，最终我们一定会输（因为我们必会拿1）。

Case 2: 我们拿到的是一个偶数，我们每次选择1，逼对方去拿奇数，最终对方一定会拿到1，我们必赢。

```go
//leetcode submit region begin(Prohibit modification and deletion)
//执行耗时:0 ms,击败了100.00% 的Go用户
//内存消耗:1.9 MB,击败了97.56% 的Go用户
func divisorGame(N int) bool {
	//如果是偶数
	if N & 1 == 0 {
		return true  //对方最终一定拿到一个奇数，因此你会赢
	}

	//如果是奇数
	return false
}
```

