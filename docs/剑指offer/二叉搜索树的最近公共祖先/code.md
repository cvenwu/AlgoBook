# [剑指 Offer 68 - I. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

## 方法一：
!> 思路如下：
1. 如果根节点为空，并且传入的两个节点的值有一个与我们的根节点的值相等，直接返回root
2. 如果两个节点都比根小，则递归到根节点左侧，若两个节点都比根大，则递归到根节点右侧，否则(说明一个比根节点小一个比根节点的值大)返回根节点
   
```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
	//如果根为空，说明没有公共节点返回nil
	//如果root的值等于p的值或者等于q的值，直接返回root
	//上面两种情况总和之后就是这样写
	if root == nil || root.Val == p.Val || root.Val == q.Val {
		return root
	}

	//如果都比根小
	if root.Val > p.Val && root.Val > q.Val {
		return lowestCommonAncestor(root.Left, p, q)
	} else if root.Val < p.Val && root.Val < q.Val { //如果都比根大
		return lowestCommonAncestor(root.Right, p, q)
	} else { //如果一个比根大，一个比根小
		return root
	}
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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        //如果两个树节点有一个等于根的值，或者一个比root大一个比root小，直接返回
        if (root->val == p->val || root->val == q->val)
        {
            return root;
        } 
        // /如果两个都比root小则，递归遍历到左子树
        else if (root->val > p->val && root->val > q->val)  
        {
            return this->lowestCommonAncestor(root->left, p, q);
        }
        else if (root->val < p->val && root->val < q->val)  
        {
            return this->lowestCommonAncestor(root->right, p, q);
        } 
        else
        {
            return root;
        }
    }
};
```