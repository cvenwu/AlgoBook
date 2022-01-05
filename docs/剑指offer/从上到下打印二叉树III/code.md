# 从上到下打印二叉树III
## 方法一：剑指offer思路，使用两个栈

**思路**：
1. 假设第1层为根节点，也就是奇数层
2. 我们用奇数栈保存奇数层要打印的节点，偶数栈保存偶数层要打印的节点
3. 如果当前打印的是奇数层的节点，则按照先加入该节点的左子节点到偶数栈，再加入该节点的右子节点到偶数栈
4. 如果当前打印的是偶数层的节点，则按照先加入该节点的右子节点到奇数栈，再加入该节点的左子节点到奇数栈

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

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        //如果当前节点为空，直接返回一个空的vector
        if (!root) return {};
        //结果
        vector<vector<int>> ret;
        //当前在哪一层
        int level = 0;
        //保存奇数层结果的栈
        stack<TreeNode*> oddStack;
        //保存偶数层结果的栈
        stack<TreeNode*> evenStack;
        //根节点加入偶数栈中
        evenStack.push(root);
        //临时节点
        TreeNode *temp;
        //保存当前层的结果
        vector<int> curLevelRet;

        while(!oddStack.empty() || !evenStack.empty())
        {
            //如果是奇数层
            if (level & 1) 
            {
                while(!oddStack.empty())
                {
                    //从奇数栈出来，先加入右子树再加入左子树
                    temp = oddStack.top();
                    oddStack.pop();
                    curLevelRet.push_back(temp->val);
                    if (temp->right) evenStack.push(temp->right);
                    if (temp->left) evenStack.push(temp->left);
                }
            } else {//假设根所在的是偶数层，偶数层先加入左子树，再加入右子树
                //从偶数栈中弹出所有节点并加入子节点到奇数层中
                while(!evenStack.empty())
                {
                    temp = evenStack.top();
                    evenStack.pop();
                    curLevelRet.push_back(temp->val);
                    if (temp->left) oddStack.push(temp->left);
                    if (temp->right) oddStack.push(temp->right);
                }
            }
            level++;
            ret.push_back(curLevelRet);
            curLevelRet.clear();
        }

        return ret;
    }
};
```

## 方法2：对层次遍历中的部分层进行反转
!> 也就是加入当前层结果的时候判断当前层是否需要反转(满足题目中的条件)，反转之后加入到最终的结果


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

void reverseVector(vector<int> &v)
{
    for (int i = 0; i < v.size()>>1; i++)
    {
        v[i] ^= v[v.size()-1-i];
        v[v.size()-1-i] ^= v[i];
        v[i] ^= v[v.size()-1-i];
    }
}

class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        //如果当前节点为空，直接返回一个空的vector
        if (!root) return {};
        //结果
        vector<vector<int>> ret;
        //队列
        queue<TreeNode*> q;
        //加入根节点
        q.push(root);
        //记录当前层元素个数
        int curLevelSize;
        //记录当前层的结果
        vector<int> curLevelRet;
        //保存临时的节点
        TreeNode *temp;
        //当前是第几层，从0开始
        int level = 0;
        //遍历队列
        while(!q.empty())
        {
            curLevelSize = q.size();
            for (int i = 0; i < curLevelSize; i++)
            {
                //第一个元素
                temp = q.front();
                //弹出第一个元素
                q.pop();
                //左子树
                if (temp->left) q.push(temp->left);
                //右子树
                if (temp->right) q.push(temp->right);
                //当前节点加入当前层
                curLevelRet.push_back(temp->val);
            }

            //反转奇数层
            if (level & 1)
                reverseVector(curLevelRet);
            level++;
            //将当前层的结果加入到最终的结果汇总
            ret.push_back(curLevelRet);
            //清空当前层的结果
            curLevelRet.clear();
        }
            
        return ret;
    }
};
```


## 方法三：遍历之后反转奇数层
!> 先将当前层的结果加入到结果中，然后反转最终结果中需要反转的层
```go
func levelOrder(root *TreeNode) [][]int {
	if root == nil {
		return nil
	}

	queue := []*TreeNode{root}
	var ret [][]int
	for len(queue) != 0 {
		length := len(queue)
		curLevel := make([]int, length)
		for i := 0; i < length; i++ {
			node := queue[0]
			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
			curLevel[i] = node.Val
			queue = queue[1:]
		}
		ret = append(ret, curLevel)
	}

	//获得层序遍历的结果后，如果是奇数就将其反转
	for i := 1; i < len(ret); i += 2 {
		length := len(ret[i])
		for j := 0; j < length>>1; j++ {
			ret[i][j], ret[i][length-1-j] = ret[i][length-1-j], ret[i][j]
		}
	}
	return ret
}
```

```c++

void reverseVector(vector<int> &v)
{
    for (int i = 0; i < v.size()>>1; i++)
    {
        v[i] ^= v[v.size()-1-i];
        v[v.size()-1-i] ^= v[i];
        v[i] ^= v[v.size()-1-i];
    }
}

class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        //如果当前节点为空，直接返回一个空的vector
        if (!root) return {};
        //结果
        vector<vector<int>> ret;
        //队列
        queue<TreeNode*> q;
        //加入根节点
        q.push(root);
        //记录当前层元素个数
        int curLevelSize;
        //记录当前层的结果
        vector<int> curLevelRet;
        //保存临时的节点
        TreeNode *temp;
        //遍历队列
        while(!q.empty())
        {
            curLevelSize = q.size();
            for (int i = 0; i < curLevelSize; i++)
            {
                //第一个元素
                temp = q.front();
                //弹出第一个元素
                q.pop();
                //左子树
                if (temp->left) q.push(temp->left);
                //右子树
                if (temp->right) q.push(temp->right);
                //当前节点加入当前层
                curLevelRet.push_back(temp->val);

            }

            //将当前层的结果加入到最终的结果汇总
            ret.push_back(curLevelRet);
            //清空当前层的结果
            curLevelRet.clear();
        }
        for (int i = 1; i < ret.size(); i+=2)
            reverseVector(ret[i]);
        return ret;
    }
};
```