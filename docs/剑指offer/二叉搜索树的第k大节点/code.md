# 二叉搜索树的第k大节点

思路：中序遍历后返回倒数第k个节点



> // 执行用时：16 ms, 在所有 Go 提交中击败了 30.26% 的用户
> 		// 内存消耗：6.3 MB, 在所有 Go 提交中击败了 25.00% 的用户



```go
package main

import "fmt"

//思路：返回中序遍历生成的序列中的第k个节点

func InorderTraverse(root *TreeNode, ret *[]int) {
	if root == nil {
		return 
	}

	InorderTraverse(root.Left, ret)
	*ret = append(*ret, root.Val)
	InorderTraverse(root.Right, ret)
}

func kthLargest(root *TreeNode, k int) int {
	ret := &[]int{}
	InorderTraverse(root, ret)
    //说明没找到第k大节点
	if k < 1 && k > len(*ret) {
		return -1
	}

    return (*ret)[len(*ret) - k]
}
```

