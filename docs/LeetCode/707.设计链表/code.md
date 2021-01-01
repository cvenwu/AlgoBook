# [707.设计链表](https://leetcode-cn.com/problems/design-linked-list/)


## 方法一：

!> 注意：如果要插入的索引等于我们的长度，则插入尾部的最后一个元素


```go
package main

/**
 * @Author: yirufeng
 * @Date: 2020/12/30 9:31 上午
 * @Desc:
 **/


//执行用时：36 ms, 在所有 Go 提交中击败了32.91%的用户
//内存消耗：6.9 MB, 在所有 Go 提交中击败了87.34%的用户
//type ListNode struct {
//	Val        int
//	Next, Prev *ListNode
//}

type MyLinkedList struct {
	headDummyNode *ListNode
	tailDummyNode *ListNode
	Length        int
}

/** Initialize your data structure here. */
func Constructor() MyLinkedList {

	headDummyNode := &ListNode{}
	tailDummyNode := &ListNode{}
	headDummyNode.Next, headDummyNode.Prev, tailDummyNode.Next, tailDummyNode.Prev = tailDummyNode, tailDummyNode, headDummyNode, headDummyNode
	return MyLinkedList{
		headDummyNode: headDummyNode,
		tailDummyNode: tailDummyNode,
		Length:        0,
	}
}

//一个公共函数，表示将第二个参数中的节点插入到第1个参数后面的节点
func insertAfterNode(prev *ListNode, node *ListNode) {
	prev.Next, prev.Next.Prev, node.Next, node.Prev = node, node, prev.Next, prev
}

/** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
func (this *MyLinkedList) Get(index int) int {
	//索引无效
	if this.Length < index+1 {
		return -1
	}

	if this.Length == index+1 {
		return this.tailDummyNode.Prev.Val
	}

	//开始走index+1步然后返回对应节点的值即可
	cur := this.headDummyNode
	for i := 0; i < index+1; i++ {
		cur = cur.Next
	}

	return cur.Val
}

/** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
func (this *MyLinkedList) AddAtHead(val int) {

	node := &ListNode{Val: val}
	insertAfterNode(this.headDummyNode, node)

	this.Length++

}

/** Append a node of value val to the last element of the linked list. */
func (this *MyLinkedList) AddAtTail(val int) {

	node := &ListNode{Val: val}

	insertAfterNode(this.tailDummyNode.Prev, node)
	this.Length++

}

/** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
func (this *MyLinkedList) AddAtIndex(index int, val int) {

	//尾部插入
	//TODO:第一次犯错出现在这里，如果要插入的索引等于我们的长度，则插入尾部的最后一个元素
	if index == this.Length {
		this.AddAtTail(val)
		return
	}

	//否则新建节点，然后找到插入位置，执行insertAfterNode
	node := &ListNode{Val: val}
	cur := this.headDummyNode
	for i := 0; i < index; i++ {
		cur = cur.Next
	}
	//此时cur为要插入的节点的上一个节点

	insertAfterNode(cur, node)
	this.Length++

}

/** Delete the index-th node in the linked list, if the index is valid. */
func (this *MyLinkedList) DeleteAtIndex(index int) {

	//说明索引无效
	//TODO：注意点 注意这里是或而不是与
	if index < 0 || index >= this.Length {
		return
	}

	//从head走index步走到要删除节点的上一个节点
	prev := this.headDummyNode
	for i := 0; i < index; i++ {
		prev = prev.Next
	}

	prev.Next, prev.Next.Next.Prev, prev.Next.Next, prev.Next.Prev = prev.Next.Next, prev, nil, nil
	this.Length--
}



/**
 * Your MyLinkedList object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.Get(index);
 * obj.AddAtHead(val);
 * obj.AddAtTail(val);
 * obj.AddAtIndex(index,val);
 * obj.DeleteAtIndex(index);
 */

```