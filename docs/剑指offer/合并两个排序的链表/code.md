# 合并两个排序的链表

## 方法一：迭代

!> 技巧：因为不确定链表的头结点是两个链表中的哪一个，所以我们可以先建立一个虚拟头节点

```go
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {

	dummyNode := &ListNode{}
	cur := dummyNode
	cur1, cur2 := l1, l2
	for cur1 != nil && cur2 != nil {
		if cur1.Val < cur2.Val {
			cur.Next, cur1.Next, cur1 = cur1, nil, cur1.Next
		} else {
			cur.Next, cur2.Next, cur2 = cur2, nil, cur2.Next
		}

		cur = cur.Next
	}

	if cur1 != nil {
		cur.Next = cur1
	}

	if cur2 != nil {
		cur.Next = cur2
	}

	return dummyNode.Next
}
```


```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {

        auto dummyNode = new ListNode();
        auto cur = dummyNode;
        while(l1 && l2)
        {
            //谁小谁先放入
            if (l1->val <= l2->val)
            {
                cur->next = l1;
                l1 = l1->next;
                cur = cur->next;
            } else {
                cur->next = l2;
                l2 = l2->next;
                cur = cur->next;
            }
        }

        if (l1) cur->next = l1;
        if (l2) cur->next = l2;
        return dummyNode->next;
    }
};
```

## 方法二：递归

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    if l1 == nil {
        return l2
    }

    if l2 == nil {
        return l1
    }

    //此时两个都不为空
    if l1.Val <= l2.Val {
        //此时l1之后的都已经从小到大有序了
        l1.Next = mergeTwoLists(l1.Next, l2)
        return l1
    }

    l2.Next = mergeTwoLists(l1, l2.Next)
    return l2
}
```

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (!l1) return l2;
        if (!l2) return l1;

        //找到小的然后返回
        if (l1->val < l2->val)
        {
            l1->next = mergeTwoLists(l1->next, l2);
            return l1;
        }

        l2->next = mergeTwoLists(l1, l2->next);
        return l2;
    }
};
```