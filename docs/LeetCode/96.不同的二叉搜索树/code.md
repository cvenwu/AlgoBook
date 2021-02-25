# [96.不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)

## 方法：动态规划

> 为什么使用dp:
1. 题目中看了最后是求最值，所以我们可以尝试是否可以使用dp
2. 发现后面的状态都是由前面推导出来的，如果不用就有大量重复记录

1. **状态：**G(i)表示i+1个节点可以组成多少个bst
2. **转移方程：** G(n) = G(0)\*G(n-1) + ... +G(n-1)\*G(0)
3. **初始状态：** G[0]=1，是一个特殊状态，后面都可以由转移方程推导出来

![h=3](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/h=3.png)

```go
func numTrees(n int) int {
	G := make([]int, n+1)
	G[0] = 1
	for i := 1; i <= n; i++ {  //假设G(i)有i+1个点
		for j := 0; j <= i-1; j++ {  //左子树可以有0~i-1个节点
			G[i] += G[j] * G[i-j-1]
		}
	}
	return G[n]
}
```