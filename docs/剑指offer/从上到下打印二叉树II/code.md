# 从上到下打印二叉树II

## 方法一：使用双重循环

**这里利用了一个小技巧：我们每次使用len(queue)就可以获取当前层的个数**

例如：最开始只有一个根节点，当弹出根节点，加入两个子节点之后，使用len()又获取到了第2层的节点个数，以此类推

> *// 执行用时：0 ms, 在所有 Go 提交中击败了100.00% 的用户*
>
> *// 内存消耗：2.8 MB, 在所有 Go 提交中击败了 47.4% 的用户*

```go
func levelOrder(root *TreeNode) [][]int {
	//如果根节点为空
	if root == nil {
		return nil
	}

	queue := []*TreeNode{root}
	ret := [][]int{}

	for len(queue) != 0 {
		currentLevelRet := []int{}
		length := len(queue)

		for i := 0; i < length; i++ {
			node := queue[0]
			currentLevelRet = append(currentLevelRet, node.Val)
			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
			queue = queue[1:]
		}
		ret = append(ret, currentLevelRet)
	}
	return ret
}
```

## 方法二：使用两个变量 (剑指offer思路)

**思路：使用两个变量toBePrinted, nextLevel 分别表示当前层剩余的待打印节点的个数以及下一层要打印的个数，当toBePrinted等于0时，说明当前层打印完成，添加当前层的结果到最终的结果中，同时将nextLevel赋值给toBePrinted之后将nextLevel赋值0，开始下一层**

> *// 执行用时：0 ms, 在所有 Go 提交中击败了100.00% 的用户*
>
> *// 内存消耗：2.8 MB, 在所有 Go 提交中击败了 37.04% 的用户*

```go
func levelOrder(root *TreeNode) [][]int {
	//如果根节点为空
	if root == nil {
		return nil
	}

	queue := []*TreeNode{root}
	ret := [][]int{}
	toBePrinted, nextLevel := 1, 0
	temp := []int{}
	for toBePrinted != 0 {
		temp, toBePrinted = append(temp, queue[0].Val), toBePrinted-1
		if queue[0].Left != nil {
			queue = append(queue, queue[0].Left)
			nextLevel += 1
		}
		if queue[0].Right != nil {
			queue = append(queue, queue[0].Right)
			nextLevel += 1
		}
		queue = queue[1:]
		if toBePrinted == 0 { //表明当前层打完
			toBePrinted = nextLevel
			nextLevel = 0
			ret = append(ret, temp)
			temp = []int{}
		}
	}
	return ret
}
```

## 方法三：左神

!> 思路：使用两个变量，一个是当前层的最后一个节点，以及遍历过程中不断动态更新的下一层最后一个节点

```go
//使用两个变量，表示当前行最后一个以及下一行最后一个
func levelOrder(root *TreeNode) [][]int {
	if root == nil {
		return nil
	}
	var ret [][]int
	last := root
	var nextLast *TreeNode
	queue := []*TreeNode{last}

	curLevel := []int{}
	for len(queue) != 0 {
		node := queue[0]
		curLevel = append(curLevel, node.Val)
		if node.Left != nil {
			queue = append(queue, node.Left)
			nextLast = node.Left
		}
		if node.Right != nil {
			queue = append(queue, node.Right)
			nextLast = node.Right
		}
		queue = queue[1:]

		//说明要开始换行
		if node == last {
			ret = append(ret, curLevel)
			curLevel = []int{}
			last = nextLast
		}
	}

	return ret
}
```

