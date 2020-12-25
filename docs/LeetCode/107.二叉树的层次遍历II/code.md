# [107. 二叉树的层次遍历 II](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)

关键点：

1. 如何实现层次遍历：**BFS或DFS**
2. 层次遍历之后如何进行反转，两个方法，
   1. 第1个方法是遍历完之后进行反转，
   2. 第2个方法就是**每次遍历当前层的时候先将当前层先加入到ret中，再将ret之前的层次再加入进来**

## 方法一：bfs+反转


> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.8 ms, 在所有 Go 提交中击败了58.80%的用户


```go
func levelOrder(root *TreeNode) [][]int {
	queue := []*TreeNode{root}
	ret := [][]int{}
	for len(queue) != 0 {
		length := len(queue)
		currentLevelRet := []int{}
		for length > 0 {
			node := queue[0]
			queue = queue[1:]
			length -= 1
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

func reverse(ret [][]int) [][]int {
	for i := 0; i < len(ret) >> 1; i++ {
		ret[i], ret[len(ret)-i-1] = ret[len(ret)-1-i], ret[i]
	}
	return ret
}
```

## 方法二：bfs+每次加入当前层元素的时候先加入再加入之前的层的结果

如何实现反转：每次加入当前层元素的时候先将之前加入的剔除掉，然后加入当前层，再加入剔除掉的之前层的结果

```go
func levelOrderBottom(root *TreeNode) [][]int {
	var res [][]int
	if root==nil{
		return res
	}
	queue := []*TreeNode{root}
	for len(queue)>0{
		l :=len(queue)
		list := make([]int,0)
		for i:=0;i<l;i++{
			node :=queue[i]
			list = append(list,node.Val)
			if node.Left!=nil{
				queue = append(queue,node.Left)
			}
			if node.Right!=nil{
				queue = append(queue,node.Right)
			}
		}
		//本方法的核心
		res = append([][]int{list},res...)
		queue =queue[l:]
	}
	return res
}

```

## 方法三：DFS+反转

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：3.1 ms, 在所有 Go 提交中击败了39.33%的用户

```go
func levelOrderBottom(root *TreeNode) [][]int {
   if root == nil {
      return nil
   }
   ret := [][]int{}
   dfs(root, 1, &ret)
   reverse(&ret)
   return ret
}

func reverse(ret *[][]int) {
   for i := 0; i < len(*ret) >> 1; i++ {
      (*ret)[i], (*ret)[len(*ret)-i-1] = (*ret)[len(*ret)-1-i], (*ret)[i]
   }
}


func dfs(root *TreeNode, level int, ret *[][]int) {
   if len(*ret) < level {
      *ret = append(*ret, []int{})
   }

   (*ret)[level-1] = append((*ret)[level-1], root.Val)
   if root.Left != nil {
      dfs(root.Left, level+1, ret)
   }
   if root.Right != nil {
      dfs(root.Right, level+1, ret)
   }
}
```