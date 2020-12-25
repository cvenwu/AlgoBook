# 从上到下打印二叉树III



## 方法一：剑指offer思路，使用两个栈

**思路**：

1. 假设第1层为根节点，也就是奇数层

2. 我们用奇数栈保存奇数层要打印的节点，偶数栈保存偶数层要打印的节点

3. 如果当前打印的是奇数层的节点，则按照先加入该节点的左子节点到偶数栈，再加入该节点的右子节点到偶数栈

4. 如果当前打印的是偶数层的节点，则按照先加入该节点的右子节点到奇数栈，再加入该节点的左子节点到奇数栈



> *// 执行用时：0 ms, 在所有 Go 提交中击败了100.00% 的用户*
>
> *// 内存消耗：2.7 MB, 在所有 Go 提交中击败了 100.0% 的用户*

```go
func levelOrder(root *TreeNode) [][]int {
	if root == nil {
		return nil
	}
	flag := true //表明是奇数栈
	ret := [][]int{}
	oddStack, evenStack := []*TreeNode{root}, []*TreeNode{}
	for len(oddStack) != 0 || len(evenStack) != 0 {
		currentLevelRet := []int{}
		if flag {
			for len(oddStack) != 0 {
				node := oddStack[len(oddStack)-1]
				currentLevelRet = append(currentLevelRet, node.Val)
				if node.Left != nil {
					evenStack = append(evenStack, node.Left)
				}
				if node.Right != nil {
					evenStack = append(evenStack, node.Right)
				}
				oddStack = oddStack[:len(oddStack)-1]
			}
		} else {
			for len(evenStack) != 0 {
				node := evenStack[len(evenStack)-1]
				currentLevelRet = append(currentLevelRet, node.Val)

				if node.Right != nil {
					oddStack = append(oddStack, node.Right)
				}
				if node.Left != nil {
					oddStack = append(oddStack, node.Left)
				}
				evenStack = evenStack[:len(evenStack)-1]
			}
		}
		flag = !flag
		ret = append(ret, currentLevelRet)
	}
	return ret

}
```

## 方法2：对层次遍历中的部分层进行反转

假设根节点在第0层，我们需要在奇数层遍历完之后将反转的结果加入到最终的结果中

> *// 执行用时：0 ms, 在所有 Go 提交中击败了100.00% 的用户*
>
> *// 内存消耗：2.8 MB, 在所有 Go 提交中击败了 57.89% 的用户*

```go
func levelOrder(root *TreeNode) [][]int {
	//如果根节点为空
	if root == nil {
		return nil
	}

	queue := []*TreeNode{root}
	ret := [][]int{}
	level := 1
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
		if level&1 != 1 { //如果是偶数层，则进行反转
			currentLevelRet = reverse(currentLevelRet)
		}
		level += 1
		ret = append(ret, currentLevelRet)
	}
	return ret
}

func reverse(nums []int) []int {
	if len(nums) == 0 {
		return nil
	}
	for i, j := 0, len(nums)-1; i < j; i, j = i+1, j-1 {
		nums[i], nums[j] = nums[j], nums[i]
	}
	return nums
}
```

