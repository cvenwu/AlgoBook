# [605. 种花问题](https://leetcode-cn.com/problems/can-place-flowers/)

## 方法一：贪心


!> 思路：
	1. 找到可以插入的最大的花的数目，如果n小于等于我们的count则直接返回true否则为false
	2. 因此转换为如何找到可以插入的最大的花的数目，我们使用贪心，也就是**前面一个元素如果有为0，当前元素也为0，后面元素如果有的话为0，说明当前元素就可以插入一个，count++，并且将当前位置置1
		否则我们就后移一个单位**
	3. 如果前面一个数字为0或者前面没有数，且当前数字为0，且后面一个数字没有或者后面一个数字为0，那么当前一定可以插入1，所以就加一，同时count++

> 执行用时：20 ms, 在所有 Go 提交中击败了74.11%的用户
> 内存消耗：6 MB, 在所有 Go 提交中击败了88.85%的用户

```go
func canPlaceFlowers(flowerbed []int, n int) bool {
	count, prev, cur, next := 0, -1, 0, 1

	for cur != len(flowerbed) {
		if (prev == -1 || flowerbed[prev]==0) && (flowerbed[cur] == 0) && (next == len(flowerbed) || flowerbed[next] == 0) {
			flowerbed[cur] = 1
			count++
		}
		prev, cur, next = prev+1, cur+1, next+1
	}

	if count >= n {
		return true
	}

	return false
}
```