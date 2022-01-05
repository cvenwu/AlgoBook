# 从上到下打印二叉树


**思路：直接使用 BFS 即可**

## 方法一：使用container/list

```go
// 执行用时：0 ms, 在所有 Go 提交中击败了100.00% 的用户
// 内存消耗：2.7 MB, 在所有 Go 提交中击败了 100.00% 的用户
func levelOrder(root *TreeNode) []int {
	//如果根节点为空
	if root == nil {
		return []int{}
	}

	queue := list.New()
	queue.PushBack(root)
	var ret []int

	for queue.Front() != nil {
		node := queue.Remove(queue.Front())
		ret = append(ret, node.(*TreeNode).Val)
		if node.(*TreeNode).Left != nil {
			queue.PushBack(node.(*TreeNode).Left)
		}
		if node.(*TreeNode).Right != nil {
			queue.PushBack(node.(*TreeNode).Right)
		}
	}

	return ret
}
```

## 【推荐】方法二： 使用slice

```go

// 执行用时：0 ms, 在所有 Go 提交中击败了100.00% 的用户
// 内存消耗：2.7 MB, 在所有 Go 提交中击败了 100.00% 的用户

func levelOrder(root *TreeNode) []int {
	//如果根节点为空
	if root == nil {
		return nil
	}

	queue := []*TreeNode{root}
	ret := []int{}

	for len(queue) != 0 {
		node := queue[0]
		ret = append(ret, node.Val)
		if node.Left != nil {
			queue = append(queue, node.Left)
		}
		if node.Right != nil {
			queue = append(queue, node.Right)
		}
		//注意：一定要记得加上这句话，不然会死循环
		queue = queue[1:]
	}
	return ret
}
```

```c++
class Solution {
public:
    vector<int> levelOrder(TreeNode* root) {
        if (!root)
            return {};

        //建立一个队列
        queue<TreeNode*> q;
        //将我们当前节点加入到队列中
        q.push(root);
        
        vector<int> ret;

        TreeNode *temp;
        //循环遍历队列
        while(q.size())
        {
            temp = q.front();//返回队头的第一个节点
            q.pop(); //移除第一个节点
            
            if (temp->left)//如果左子树存在
                q.push(temp->left);
            
            if (temp->right)//如果右子树存在
                q.push(temp->right);
            ret.push_back(temp->val);
        }
        return ret;
    }
};
```
