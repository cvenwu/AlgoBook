# 二叉树的镜像

## 【推荐】方法一：递归
!>  思路：每次不断反转当前节点的左右子树，直到当前节点为空
```go
func mirrorTree(root *TreeNode) *TreeNode {
	if root == nil {
		return nil
	}

	root.Left, root.Right = mirrorTree(root.Right), mirrorTree(root.Left)
	return root
}
```

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* mirrorTree(TreeNode* root) {
        //如果是空指针直接返回空指针
        if (root == nullptr) return nullptr;
        
        //反转之后的左子树
        TreeNode *left = this->mirrorTree(root->left);
        //保存反转之后的右子树
        TreeNode *right = this->mirrorTree(root->right);
        //将反转之后的左子树转接到当前节点的右子树中
        root->left = right;
        //将反转之后的右子树转接到当前节点的左子树中
        root->right = left;
        //最后返回当前节点
        return root;
    }
};
```

## 方法二：采用先序遍历反转

**思路：**
1. 采用先序遍历的方式，先弹出队列的队首，然后将当前节点的左右反转
2. 如果当前节点的左边不为空，则将当前节点的左边加入队列
3. 如果当前节点的右边不为空，则将当前节点的右边加入队列


```go
func mirrorTree(root *TreeNode) *TreeNode {
	if root == nil {
		return nil
	}

	queue := []*TreeNode{}

	queue = append(queue, root)

	for len(queue) > 0 {
		node := queue[0]
		queue = queue[1:]
		//反转node的左右
		node.Left, node.Right = node.Right, node.Left
		//如果左不为空，则加入队列
		if node.Left != nil {
			queue = append(queue, node.Left)
		}
		//如果右不为空，则加入队列
		if node.Right != nil {
			queue = append(queue, node.Right)
		}
	}
	return root
}
```

