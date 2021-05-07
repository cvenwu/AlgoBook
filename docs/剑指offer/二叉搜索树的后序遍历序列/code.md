# 二叉搜索树的后序遍历序列

## 方法一：
思路：
1. 找到根节点
2. 从我们的序列中找到第一个大于等于根节点的，这个节点一定是我们的右子树的开头
3. 然后从第一个大于等于我们根节点的节点开始遍历一直到根节点前面一个节点，如果有一个是小于我们的根节点，我们就直接返回false
4. 之后，我们继续遍历我们的左子树和右子树

```go
func verifyPostorder(postorder []int) bool {
	//如果长度为0说明就不是BST
	//注意点1：长度为0的时候也是true
	if len(postorder) == 0 {
		return true
	}
	//返回我们的Core函数
	return verifyPostorderCore(postorder, 0, len(postorder)-1)
}

//l,r分别表示当前后序遍历位于我们给定切片的起点和终点
func verifyPostorderCore(postorder []int, l, r int) bool {

	//l==r的时候一定满足我们的BST
	//l>r的时候此时既然能够遍历到l>r的情况，说明之前遍历的时候都是满足我们的条件，这里我们暂时返回true，让它再进行判断
	if l >= r {
		return true
	}

	//找到最后一个节点作为根节点
	val := postorder[r]

	//然后从后序遍历的l开始，直到找到第一个比它大的
	i := l
	for ; i <= r; i++ {
		if postorder[i] >= val {
			break
		}
	}
	//检测i右边的是否都是大于我们的根节点的值
	for j := i; j < r; j++ {
		if postorder[j] < val {
			return false
		}
	}

	//这里我们需要继续递归验证左右子树是否满足我们的条件：不能光验证我们的左子树
	return verifyPostorderCore(postorder, l, i-1) && verifyPostorderCore(postorder, i, r-1)
}
```