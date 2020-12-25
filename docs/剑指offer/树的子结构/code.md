# [剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

## 方法一：DFS

>  参考剑指offer代码写的代码
> 		 思路：递归

1. 首先判断当前A节点的值是否与b节点的值相等，如果相等执行判断结构是否相同的函数
2. 如果不等，则递归到A的左子树是否包括b的结构 或 递归到A的右子树是否包括b的结构


> 执行用时：24 ms, 在所有 Go 提交中击败了 95.45% 的用户
		内存消耗：7.3 MB, 在所有 Go 提交中击败了 13.54% 的用户

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func isSubStructure(A *TreeNode, B *TreeNode) bool {
	ret := false
	if A != nil && B != nil {
		//如果两个节点的值相同
		if A.Val == B.Val {
			ret = hasSameSubTreeStructure(A, B)
		}
		//如果前面判断了A和B根相同是结构不同返回false,那么这次遍历左子树
		if !ret {
			ret = isSubStructure(A.Left, B)
		}
		if !ret {
			ret = isSubStructure(A.Right, B)
		}
	}

	return ret
}

//B左右两个分支如果有，A一定要有且值要和B分支值相等
func hasSameSubTreeStructure(A, B *TreeNode) bool {

	//如果B空，返回true
	if B == nil {
		return true
	}

	//遍历到这里说明B不空，但此时A空了，直接返回false即可
	if A == nil {
		return false
	}

	//两个节点都有值但是值不等，返回false
	if A.Val != B.Val {
		return false
	}

	//后面继续递归判断
	return hasSameSubTreeStructure(A.Left, B.Left) && hasSameSubTreeStructure(A.Right, B.Right)

}
```

