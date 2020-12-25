# [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

## 方法一：从上到下递归

思路：对于给定的root，如果该节点的左右子节点相等，那么继续判断该节点的左子树与右子树是否镜像对称

> 执行用时：4 ms, 在所有 Go 提交中击败了75.56%的用户
> 		内存消耗：2.9 MB, 在所有 Go 提交中击败了71.38%的用户

```go
func isSymmetric(root *TreeNode) bool {
	if root == nil {
		return true
	}
	return isSymmetricCore(root.Left, root.Right)
}

//检查left与right是否镜像对称
func isSymmetricCore(left *TreeNode, right *TreeNode) bool {
	//如果两个都为空
	if left == nil && right == nil {
		return true
	}
	//如果有一个为空
	if left == nil || right == nil {
		return false
	}
	//如果两个的值相等，查看子树是否镜像对称
	if left.Val == right.Val {
		return isSymmetricCore(left.Left, right.Right) &&
			isSymmetricCore(left.Right, right.Left)
	}
	//两个的值不相等
	return false
}
```

## 方法二：从下到上递归

思路：当左右子树都存在且对称的情况下，需要判断当前节点的左右子节点是否值相等

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.9 MB, 在所有 Go 提交中击败了71.38%的用户

```go
func isSymmetric(root *TreeNode) bool {
	if root == nil {
		return true
	}
	return isSymmetricCore(root.Left, root.Right)
}

//检查left与right是否镜像对称
func isSymmetricCore(left *TreeNode, right *TreeNode) bool {
	//如果两个都为空
	if left == nil && right == nil {
		return true
	}
	//如果有一个为空
	if left == nil || right == nil {
		return false
	}
	//如果两个的值相等，查看子树是否镜像对称
	if isSymmetricCore(left.Left, right.Right) &&
		isSymmetricCore(left.Right, right.Left) {
		return left.Val == right.Val
	}
	//两个子树不对称
	return false
}
```

## 方法三：对方法一和方法二代码的优化

改进：对方法一二进行代码优化，如果左右子树都存在的情况下，只需要判断两个左右子树是否值相等并且是否左右子树自己的左右子树是否镜像对称

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.9 MB, 在所有 Go 提交中击败了24.34%的用户

```go
func isSymmetric(root *TreeNode) bool {
	if root == nil {
		return true
	}
	return isSymmetricCore(root.Left, root.Right)
}

//检查left与right是否镜像对称

func isSymmetricCore(left *TreeNode, right *TreeNode) bool {
	//如果两个都为空
	if left == nil && right == nil {
		return true
	}
	//如果有一个为空
	if left == nil || right == nil {
		return false
	}
	//如果两个的值相等并且子树是镜像对称，就返回true
	return isSymmetricCore(left.Left, right.Right) &&
		isSymmetricCore(left.Right, right.Left) && left.Val == right.Val
}
```



## 方法四：广度优先搜索验证是否对称

思路：bfs验证左右子树是否镜像对称，该思路参考了LeetCode官方解法

>执行用时：4 ms, 在所有 Go 提交中击败了75.56%的用户
>		内存消耗：3 MB, 在所有 Go 提交中击败了9.21%的用户

```go
func isSymmetric(root *TreeNode) bool {
	if root == nil {
		return true
	}

	queue := []*TreeNode{root, root}

	for len(queue) != 0 {
		node1, node2 := queue[0], queue[1]
		queue = queue[2:]  //这个不可以放在for循环最后，因为一定要放在continue之前
		//两个都为Nil
		if node1 == nil && node2 == nil {
			continue
		}
		//有一个为nil
		if node1 == nil || node2 == nil {
			return false
		}

		//两个的节点值不等
		if node1.Val != node2.Val {
			return false
		}
		queue = append(queue, node1.Left, node2.Right)
		queue = append(queue, node1.Right, node2.Left)
	}
	return true
}

```

