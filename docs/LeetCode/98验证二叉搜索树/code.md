# 98 验证二叉搜索树

![7304bN](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/7304bN.png)

题目中说明了根节点要大于左子树的最大节点，并没有等于，根节点要小于右子树的最小节点值，并没有等于



## 方法一：判断中序遍历序列是否升序



> 思路1：验证中序遍历是升序序列

> //执行用时：12 ms, 在所有 Go 提交中击败了12.07%的用户
> 		//内存消耗：6.9 MB, 在所有 Go 提交中击败了6.23%的用户

```go

func inorderTraversalRecursion(root *TreeNode) []int {
	if root == nil {
		return nil
	}

	ret := []int{}

	//先遍历左子树
	if root.Left != nil {
		ret = append(ret, inorderTraversalRecursion(root.Left)...)
	}

	//遍历当前节点
	ret = append(ret, root.Val)

	//再遍历右子树
	if root.Right != nil {
		ret = append(ret, inorderTraversalRecursion(root.Right)...)
	}

	return ret
}

func isValidBST(root *TreeNode) bool {
	// 空树也是bst
	if root == nil {
		return true
	}

	ret := inorderTraversalRecursion(root)
	for i := 0; i < len(ret) - 1; i++ {
		if ret[i] > ret[i+1] {
			return false
		}
	}

	return true
}
```

## 方法二：递归判断当期节点是否满足bst要求


思路：自下往上，因为自上往下有大量的重复计算
1. 如果根节点为空，直接返回true
2. 如果根节点不空，首先判断左右子树是否为bst，
          1. 如果不是，直接返回false
          2. 如果是，判断根节点大于左子树的最大值和右子树的最小值，
                1. 如果不满足，返回false
                2. 如果满足，那么更新最大值和最小值，并返回true

> //执行用时：8 ms, 在所有 Go 提交中击败了77.05%的用户
		//内存消耗：5.6 MB, 在所有 Go 提交中击败了29.76%的用户

```go
type RetType struct {
   isBST bool
   max, min *TreeNode
}

func isValidBST(root *TreeNode) bool {

   if root == nil {
      return true
   }

   return isValidBSTCore(root).isBST

}

func isValidBSTCore(root *TreeNode) RetType{
   //空树也是bst
   if root == nil {
      return RetType{isBST: true}
   }
   ret := RetType{isBST: true, min: root, max: root}

   left, right := isValidBSTCore(root.Left), isValidBSTCore(root.Right)
   //如果左右子树有1个不是平衡二叉树，那么就不是
   if !left.isBST || !right.isBST {
      ret.isBST = false
      return ret
   }

   //如果左子树不空并且根小于等于左子树的最大值，那么返回false
   if root.Left != nil && root.Val <= left.max.Val {
      ret.isBST = false
      return ret
   }

   //如果右子树不空并且根大于等于右子树的最小值，那么返回false
   if root.Right != nil && root.Val >= right.min.Val {
      ret.isBST = false
      return ret
   }

   //如果左子树不空，则更新当前最小值节点
   if root.Left != nil {
      ret.min = left.min
   }

   if root.Right != nil {
      ret.max = right.max
   }

   return ret
}
```

## 方法三：LeetCode官方题解(推荐)

[参考](https://leetcode-cn.com/problems/validate-binary-search-tree/solution/yan-zheng-er-cha-sou-suo-shu-by-leetcode-solution/)

**相比方法一二更加简短和技巧化**

```go
func isValidBST(root *TreeNode) bool {


	return helper(root, math.MinInt64, math.MaxInt64)

}

func helper(root *TreeNode, min int, max int) bool {
	if root == nil {
		return true
	}
	//此时root应该位于min, max之间
	if root.Val >= max || root.Val <= min {
		return false
	}
	//到这里说明root满足条件，再递归判断左右子树
	return helper(root.Left, min, root.Val) && helper(root.Right, root.Val, max)
}
```

