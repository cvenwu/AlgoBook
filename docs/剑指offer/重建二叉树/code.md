# 重建二叉树

## 方法一：递归建立左右子二叉树

```go
type TreeNode struct {
	Val         int
	Left, Right *TreeNode
}

func buildTree(preorder []int, inorder []int) *TreeNode {
	if len(preorder) != len(inorder) || len(preorder) == 0 || len(inorder) == 0 {
		return nil
	}
	return buildTreeCore(preorder, inorder, 0, len(preorder)-1, 0, len(inorder)-1)
}

func buildTreeCore(preorder []int, inorder []int, pStart, pEnd, inStart, inEnd int) *TreeNode {
	if pStart > pEnd || inStart > inEnd {
		return nil
	}

	//找到根的值
	rootVal := preorder[pStart]

	//找到左子树的长度
	l := 0
	for i := inStart; i <= inEnd; i++ {
		if inorder[i] == rootVal {
			l = i - 1
			break
		}
	}
	//返回当前建立的根
	return &TreeNode{
		Val:   rootVal,
		Left:  buildTreeCore(preorder, inorder, pStart+1, pStart+1+l-inStart, inStart, l),
		Right: buildTreeCore(preorder, inorder, pStart+1+l-inStart+1, pEnd, l+2, inEnd),
	}
}

```

!> C++中必须要传入一个长度来区分先序与中序的长度

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


TreeNode* buildTreeCore(vector<int>& preorder, vector<int>& inorder, int pLeft, int pRight, int inLeft, int inRight)
{
    if (pLeft > pRight || inLeft > inRight) return NULL;

    //首先找到根节点的值
    int rootVal = preorder[pLeft];
    //记录根节点的序号
    int rootIndex;
    //确定根节点在中序遍历中出现的位置
    for (rootIndex = inLeft; rootIndex <= inRight; rootIndex++ )
    {
        if (rootVal == inorder[rootIndex])
            break;
    }

    //左子树的长度
    int leftSubTreeLength = rootIndex-inLeft;

    //然后区分左右子树的序号，开始重建二叉树
    auto root = new TreeNode(rootVal);
    root->left = buildTreeCore(preorder, inorder, pLeft+1, pLeft+leftSubTreeLength, inLeft, rootIndex-1), root->right = buildTreeCore(preorder, inorder, pLeft+leftSubTreeLength+1, pRight, rootIndex+1, pRight);
    return root;
}

class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return buildTreeCore(preorder, inorder, 0, preorder.size()-1, 0, inorder.size()-1);
    }
};
```

## 【推荐】方法二：直接使用当前函数递归建立

```go
//方法二：直接使用当前函数
func buildTree(preorder []int, inorder []int) *TreeNode {
	if len(preorder) != len(inorder) || len(preorder) == 0 {
		return nil
	}

	rootVal := preorder[0]
	l := 0
	for i := 0; i < len(inorder); i++ {
		if inorder[i] == rootVal {
			l = i-1
			break
		}
	}

	return &TreeNode{
		Val: rootVal,
		Left: buildTree(preorder[1:l+2], inorder[:l+1]),
		Right: buildTree(preorder[l+2:], inorder[l+2:]),
	}

}
```