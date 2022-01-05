# 反转链表

## 方法一：迭代反转

```go
func reverseList(head *ListNode) *ListNode {
	var prev *ListNode
	cur := head
	for cur != nil {
		cur, cur.Next, prev = cur.Next, prev, cur
	}

	return prev
}
```

```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode *prev = NULL;
        ListNode *cur = head;
        //每次建立一个临时的节点保存当前节点的next指向的节点
        ListNode *next;
        while (cur)
        {
            //建立一个节点保存我们当前遍历到节点指向的下一个节点
            next = cur->next;
            //当前节点的下一个节点指针指向prev
            cur->next = prev;
            //然后将prev移动到当前的节点
            prev = cur;
            //当前节点移动到下一个节点的位置
            cur = next;
        }
        return prev;    
    }
};
```

## 方法二：递归反转
```go
func reverseList(head *ListNode) *ListNode {
	if head == nil {
		return nil
	}
	reverHead := reverseList(head.Next)
	if reverHead == nil {
		return head
	}
	head.Next.Next, head.Next = head, nil
	return reverHead
}
```


```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        //有一种情况就是当前节点为空的情况，此时需要考虑到
        if (!head || !(head->next))
        {
            return head;
        }
        //走到这里说明当前节点有下一个节点，下一个节点将作为递归反转之后链表的尾部，此时我们直接让下一个节点next到当前节点即可
        //1. 首先保存当前节点的下一个节点
        ListNode *next = head->next;
        //2. 断开当前节点与下一个节点的联系，避免反转的时候会有影响
        head->next = NULL;
        //3. 然后递归进行反转下面的，接收返回的头部，并作为返回值
        ListNode *reverseHead = reverseList(next);
        //4. 将当前节点前面保存的下一个节点的next连接到当前节点
        next->next = head;
        //5. 将第三步后面反转链表的头部直接作为返回值
        return reverseHead;
    }
};
```
