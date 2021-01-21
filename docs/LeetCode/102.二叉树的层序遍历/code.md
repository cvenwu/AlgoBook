# [102.二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)


## 方法一：
!> 使用一个长度记录当前行的节点个数
```go
//方法一：BFS第一种方法
//执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
//内存消耗：2.8 MB, 在所有 Go 提交中击败了81.17%的用户
func levelOrder(root *TreeNode) [][]int {
	if root == nil {
		return nil
	}

	var ret [][]int
	queue := []*TreeNode{root}
	for len(queue) != 0 {
		length := len(queue)
		currentLevelRet := []int{}
		for i := 0; i < length; i++ {
			node := queue[0]
			queue = queue[1:]
			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
			currentLevelRet = append(currentLevelRet, node.Val)
		}
		ret = append(ret, currentLevelRet)
	}

	return ret
}
```

## 方法二：BFS + 两个变量
!> 参考左神书籍上的思路
```go
//方法二：BFS第二种方法，使用两个变量分别代表当前层的末尾节点以及下一层的末尾节点
//执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
//内存消耗：2.8 MB, 在所有 Go 提交中击败了81.17%的用户
/*
思路：使用两个变量表示当前层的最后一个节点以及下一层的最后一个节点，
如果当前层遍历的节点到了当前层的最后一个节点，
那么开始换行，同时将下一层的最后一个节点赋值给当前层最后一个节点，
下一层最后一个节点每次跟踪记录加入队列的节点，因为刚加入的一定是最右的节点
*/
func levelOrder(root *TreeNode) [][]int {
	var ret [][]int
	queue := []*TreeNode{root}
	//当前层的最后一个节点
	curLastNode := root
	var nextLastNode *TreeNode //下一层的第一个节点

	var curLevelRet []int
	for len(queue) != 0{
		node := queue[0]
		queue = queue[1:]
		curLevelRet = append(curLevelRet, node.Val)
		if node.Left != nil {
			queue = append(queue, node.Left)
			nextLastNode = node.Left
		}
		if node.Right != nil {
			queue = append(queue, node.Right)
			nextLastNode = node.Right
		}
		//如果当前层遍历到最后一个节点，那么将开始换行
		if node == curLastNode {
			ret = append(ret, curLevelRet)
			curLastNode = nextLastNode
		}
	}

	return ret
}
```

## 方法三：DFS进行层序遍历
```go

//方法三：使用DFS进行层序遍历
//执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
//内存消耗：3 MB, 在所有 Go 提交中击败了17.90%的用户
func levelOrder(root *TreeNode) [][]int {
	if root == nil {
		return nil
	}

	var ret [][]int
	traverse(&ret, 0, root)
	return ret
}

func traverse(ret *[][]int, depth int, root *TreeNode) {
	//如果层次对应的切片没有创建
	if len(*ret)-1 < depth {
		(*ret) = append(*ret, []int{})
		//(*ret)[depth] = []int{}  //注意：这行代码会报错，因为(*ret)[depth]是一个nil
	}
	(*ret)[depth] = append((*ret)[depth], root.Val)

	if root.Left != nil {
		traverse(ret, depth+1, root.Left)
	}

	if root.Right != nil {
		traverse(ret, depth+1, root.Right)
	}
}
```
