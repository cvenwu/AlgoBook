# 序列化二叉树


## 方法一：先序遍历序列化二叉树和反序列化二叉树

!> 思路参考：[参考别人代码](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/solution/shou-hui-tu-jie-gei-chu-dfshe-bfsliang-chong-jie-f/)


```go
type Codec struct {
	delimeter string
	nilNode   string
}

func Constructor() Codec {
	return Codec{
		delimeter: ",",
		nilNode:   "#",
	}
}

// Serializes a tree to a single string.
func (this *Codec) serialize(root *TreeNode) string {
	if root == nil {
		return "#"
	}
	return strconv.Itoa(root.Val) + this.delimeter + this.serialize(root.Left) + this.delimeter + this.serialize(root.Right)
}

// Deserializes your encoded data to tree.
func (this *Codec) deserialize(data string) *TreeNode {
	//根据我们的分隔符对内容进行分割
	content := strings.Split(data, this.delimeter)
	fmt.Println(content)
	index := 0
	return this.deserializeCore(content, &index)
}

func (this *Codec) deserializeCore(data []string, index *int) *TreeNode {
	if *index >= len(data) {
		return nil
	}
	if data[*index] == this.nilNode {
		*index += 1
		return nil
	}
	rootVal, _ := strconv.Atoi(data[*index])
	root := &TreeNode{
		Val: rootVal,
	}
	*index += 1
	root.Left = this.deserializeCore(data, index)
	root.Right = this.deserializeCore(data, index)
	return root
}

/**
 * Your Codec object will be instantiated and called as such:
 * ser := Constructor();
 * deser := Constructor();
 * data := ser.serialize(root);
 * ans := deser.deserialize(data);
 */

```