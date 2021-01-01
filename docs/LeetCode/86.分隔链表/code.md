# 86. [分隔链表](https://leetcode-cn.com/problems/partition-list/)

!> 采用类似于快速排序中的划分策略进行划分即可


> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 内存消耗：2.4 MB, 在所有 Go 提交中击败了100.00%的用户

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func partition(head *ListNode, x int) *ListNode {
    if head == nil {
        return nil
    }

    //建立两个dummyNode
    lessDummy, moreEquDummy := &ListNode{}, &ListNode{}

    lessCur, moreEquCur := lessDummy, moreEquDummy

    cur := head
    for cur != nil {
        if cur.Val >= x {
            cur.Next, moreEquCur.Next, cur = nil, cur, cur.Next
            moreEquCur = moreEquCur.Next
        } else {
            cur.Next, lessCur.Next, cur = nil, cur, cur.Next
            lessCur = lessCur.Next
        }
    }

    lessCur.Next = moreEquDummy.Next
    return lessDummy.Next
}
```