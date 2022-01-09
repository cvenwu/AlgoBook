# 对称的二叉树

思路：不断的递归遍历当前节点与另一个对应位置的节点，若当前节点与对应位置的节点满足如下条件：

1. 当前节点与对应位置的节点都为空，返回true
2. 当前节点与对应位置的节点有一个为空，返回false
3. 当前节点的值与对应位置节点的值不等，返回false
4. 如果到这里说明当前节点的值与对应位置节点的值相等，此时再递归比较当前节点的左子树与对应位置节点的右子树和当前节点的右子树与对应位置节点的左子树

## 【推荐】方法一：

```go
func isSymmetric(root *TreeNode) bool {
	return isSymmetricCore(root, root)
}

func isSymmetricCore(root1, root2 *TreeNode) bool {
	if root1 == nil && root2 == nil {
		return true
	}

	if root1 == nil || root2 == nil {
		return false
	}

	return root1.Val == root2.Val && isSymmetricCore(root1.Left, root2.Right) && isSymmetricCore(root1.Right, root2.Left)
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
    bool isSymmetric(TreeNode* root) {
        return this->isSymmetricCore(root, root);
    }
private:
    bool isSymmetricCore(TreeNode* r1, TreeNode* r2) {
        if (r1 == nullptr && r2 == nullptr) return true;
        else if (r1 == nullptr || r2 == nullptr) return false;
        else return (r1->val == r2->val) && isSymmetricCore(r1->left, r2->right) && isSymmetricCore(r1->right, r2->left);
    }
};
```