# [1265. 逆序打印不可变链表](https://leetcode-cn.com/problems/print-immutable-linked-list-in-reverse/)

## 方法：递归反转即可


解题思路：可以反转链表，可以使用栈，但是题目规定不能改变原链表，并且使用栈的时候我们需要将数据压入栈中，但是题目中只有打印节点的数据没有返回节点数据的函数

```go
func printLinkedListInReverse(head ImmutableListNode) {
   //如果当前节点为空，直接进行返回
   if head == nil {
      return
   }
   //如果下一个节点为空，先递归到下一个节点之后再打印当前节点的值
   printLinkedListInReverse(head.getNext())
   head.printValue()
}
```