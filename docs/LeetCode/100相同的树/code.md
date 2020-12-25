# 100 相同的树

## 方法一：从上到下递归


> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.1 MB, 在所有 Go 提交中击败了84.70%的用户

思路：


1. 如果当前两个树的节点都不存在，则返回true
2. 如果当前两个树的节点有一个存在，则返回false
3. 如果当前两个树的节点都存在且值相等，则再判断左右子树是否相等

```go
func isSameTree(p *TreeNode, q *TreeNode) bool {

	//如果两个都为Nil
	if p == nil && q == nil {
		return true
	}

	//如果有一个为Nil返回false,
	//同时如果两个都不为空且值不等也返回nil
	if p == nil || q == nil || p.Val != q.Val {
		return false
	}

	//if p.Val != q.Val {
	//	return false
	//}

	//最后比较左右子树
	return isSameTree(p.Left, q.Left) && isSameTree(p.Right, q.Right)

}

```

## 方法二：从下到上递归



思路：需要判断从上到下递归的前提条件`前提条件是对应的左子树或右子树都有或者两个都为空`。如果满足前提条件，那么如果左右子树相同并且该节点值相同就返回true。否则返回false

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.1 MB, 在所有 Go 提交中击败了84.70%的用户

```go
//思路：如果左右子树相同并且该节点值相同就返回true，
//执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
//内存消耗：2.1 MB, 在所有 Go 提交中击败了84.70%的用户
func isSameTree(p *TreeNode, q *TreeNode) bool {

   //如果两个都为Nil
   if p == nil && q == nil {
      return true
   }

   //如果有一个为Nil返回false,
   //同时如果两个都不为空且值不等也返回nil
   if p == nil || q == nil{
      return false
   }

   //如果左右子树相同并且该节点值相同就返回true，
   if isSameTree(p.Left, q.Left)  && isSameTree(p.Right, q.Right)  && p.Val == q.Val {
      return true
   }

   return false
}
```

