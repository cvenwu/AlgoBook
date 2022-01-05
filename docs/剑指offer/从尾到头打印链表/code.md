# 从尾到头打印链表

!> 总体的思路就是涉及到了反转：借助栈 或者 递归 或者 反转已经保存链表序列的数组

## 方法一：使用栈保存我们遍历的结果

> 思路：使用栈存储遍历结果，之后再输出栈的结果。

> 衍生：既然想到使用栈，递归也是天然的栈，因此我们可以考虑使用递归
```go
//方法一：递归进行调用之后进行反转
func reversePrint(head *ListNode) []int {
	if head == nil {
		return nil
	}

	return append(reversePrint(head.Next), head.Val)
}
```

## 方法二：反转链表打印

> 思路：反转链表再打印。**需要征得面试官同意是否可以更改链表**

```go
type ListNode struct {
	Val  int
	Next *ListNode
}

func reversePrint(head *ListNode) []int {
	//反转链表
	head = reverseLinkedList(head)
	cur := head
	var ret []int
	for cur != nil {
		ret = append(ret, cur.Val)
		cur = cur.Next
	}

	//将链表反转回去
	head = reverseLinkedList(head)
	return ret
}

//返回反转后的链表头
func reverseLinkedList(head *ListNode) *ListNode {
	cur := head
	var prev *ListNode
	for cur != nil {
		cur, cur.Next, prev = cur.Next, prev, cur
	}
	return prev
}
```

```c++
ListNode* reverseLinkedList(ListNode* head)
{
    ListNode *prev = NULL;
    ListNode *cur = head;
    ListNode *next;
    while(cur)
    {
        next = cur->next;
        cur->next = prev;
        prev = cur;
        cur = next;
    }
    return prev;
}


class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        //1. 反转链表
        ListNode * reverseHead = reverseLinkedList(head);
        //2. 遍历链表
        vector<int> ret;
        ListNode *cur = reverseHead;
        while(cur)
        {
            ret.push_back(cur->val);
            cur = cur->next;
        }
        //3. 反转回去
        //此时prev是反转的头，head是尾巴
        reverseLinkedList(reverseHead);
        return ret;
    }
};
```

总结：**对于使用栈，我们可以想到递归，因为递归也是栈的一种应用场景。同时对于方法二需要修改输入的链表，我们事先需要征得面试官同意**

## 方法三：保存结果到切片中然后反转切片
!> 思路：我们知道链表中元素个数之后，创建一个结果的切片，然后正序遍历链表并且将遍历到的元素倒着添加到结果中
```go
//方法二：不需要反转链表
//思路：我们知道链表中元素个数之后，创建一个结果的切片，然后正序遍历链表并且将遍历到的元素倒着添加到结果中
func reversePrint(head *ListNode) []int {
	if head == nil {
		return nil
	}

	cur := head
	count := 0
	for cur != nil {
		count, cur = count+1, cur.Next
	}

	ret := make([]int, count)
	cur = head

	for i := count - 1; i >= 0; i-- {
		ret[i], cur = cur.Val, cur.Next
	}
	return ret
}
```

## 【推荐】方法四：遍历完链表之后将我们遍历链表得到的结果进行反转
!> 思路：其实同方法三，但是使用双指针反转我们的切片

```go
func reversePrintII(head *ListNode) []int {
	cur := head
	ret := []int{}

	for cur != nil {
		ret, cur = append(ret, cur.Val), cur.Next
	}

	count := len(ret)
	for i := 0; i < count>>1; i++ {
		ret[i], ret[count-1-i] = ret[count-1-i], ret[i]
	}

	return ret
}
```

```c++
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        //1. 遍历链表，并将遍历的结果保存到一个数组中
        ListNode *cur = head;
        vector<int> ret;
        while (cur)
        {
            ret.push_back(cur->val);
            cur = cur->next;
        }
        //2. 反转我们保存到数组中的结果
        for(int i = 0; i < ret.size()>>1; i++)
        {
            
            ret[i] ^= ret[ret.size()-1-i];
            ret[ret.size()-1-i] ^= ret[i];
            ret[i] ^= ret[ret.size()-1-i];
        }
        return ret;
    }
};
```

## 总结
| 方法           | 时间复杂度 | 空间复杂度 |
| -------------- | ---------- | ---------- |
| 方法一         | o(n)       | o(n)       |
| 方法二         | o(n)       | o(1)       |
| 方法三         | o(n)       | o(1)       |
| 【推荐】方法四 | o(n)       | o(1)       |
