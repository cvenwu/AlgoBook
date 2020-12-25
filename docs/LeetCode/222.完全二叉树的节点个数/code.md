# [222. 完全二叉树的节点个数](https://leetcode-cn.com/problems/count-complete-tree-nodes/)

完全二叉树：**根的左右两侧一定有一侧是满二叉树。此时我们可以利用2^h - 1求解该侧对应二叉树的高度**。其实完全二叉树是满二叉树与普通二叉树的结合体。

**几种情况：**

1. 左右子树高度相同：
   1.1 左子树是满二叉树，右子树是满二叉树
   1.2 左子树是满二叉树，右子树不是满二叉树
2. 左子树大于右子树高度，此时右子树一定是满二叉树

技巧：每次寻找左右子树的高度，只需要找到左子树最左边的节点以及右子树的最右边的节点，如果相等，则说明是满二叉树

## 方法一：自己写【推荐方法二，效率会高，虽然时复一样】

```go
//✅
//方法一：计算左右子树的高度，然后判断哪一侧是满的，然后递归到另外一侧非满的
//执行用时：24 ms, 在所有 Go 提交中击败了23.62%的用户
//内存消耗：7.1 MB, 在所有 Go 提交中击败了49.25%的用户
func countNodes(root *TreeNode) int {
	//如果二叉树为空，直接返回0
	if root == nil {
		return 0
	}

	//获得左右子树的高度
	hl, hr := getHeight(root.Left), getHeight(root.Right)

	//如果左右子树高度相等，说明左子树是满二叉树
	if hl == hr {
		//注意点：这里是1<<hl，不是2<<hl
		return 1<<hl + countNodes(root.Right)
	}

	//说明右子树是满二叉树
	//注意点：这里是1<<hr，不是2<<hr
	return 1<<hr + countNodes(root.Left)
}

func getHeight(root *TreeNode) int {
	if root == nil {
		return 0
	}
	return max(getHeight(root.Right), getHeight(root.Left)) + 1
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```



## 方法二：参考labuladong

```go

//✅
//方法二：参考labuladong的
//每次获取左子树的最左边节点与右子树的最右边节点的高度，如果高度相等，说明是一颗满二叉树，否则就分别递归求出左右子树的节点个数+1
//执行用时：20 ms, 在所有 Go 提交中击败了68.62%的用户
//内存消耗：7.1 MB, 在所有 Go 提交中击败了96.25%的用户
func countNodes(root *TreeNode) int {
	if root == nil {
		return 0
	}

	l, r := root.Left, root.Right
	hl, hr := 0, 0
	for l != nil {
		hl++
		l = l.Left
	}

	for r != nil {
		hr++
		r = r.Right
	}

	if hl == hr {
		return 1<<(hl+1) - 1
	}

	return 1 + countNodes(root.Left) + countNodes(root.Right)
}

```



两个方法的时间复杂度都是o(logN * logN)但是方法二常数项更加低，所以**推荐方法二**