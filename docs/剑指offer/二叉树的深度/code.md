# 二叉树的深度


## 方法一：DFS

!> 当前节点的深度 = 左右子树深度的最大值 + 1
```go
func maxDepth(root *TreeNode) int {
	if root == nil {
		return 0
	}

	//否则返回左右子树的深度的最大值+1
	return max(maxDepth(root.Left), maxDepth(root.Right)) + 1
}

func max(a, b int) int {
	if a > b {
		return a
	}

	return b
}

```

```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        //如果当前节点存在
        if (root)
            return max(this->maxDepth(root->left), this->maxDepth(root->right)) + 1;
        return 0;
    }
};
```