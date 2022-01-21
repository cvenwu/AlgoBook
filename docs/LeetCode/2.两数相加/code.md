# [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

## 方法一：迭代

!> 思路：首先计算两个链表公共部分的和，然后计算剩余链表与carry的和。

**注意：最后可能会两个链表遍历完，但是还有进位，此时我们需要新建一个节点存放进位**

```go
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
	if l1 == nil {
		return l2
	}

	if l2 == nil {
		return l1
	}


	dummyNode := &ListNode{}
	head := dummyNode
	carry := 0
	var val int
	for l1 != nil || l2 != nil {
		val = 0
		if l1 != nil {
			val += l1.Val
			l1= l1.Next
		}
		if l2 != nil {
			val += l2.Val
			l2=l2.Next
		}

		head.Next = &ListNode{
			Val: (val + carry) % 10,
		}
		carry = (val + carry) / 10
		head = head.Next
	}

	if carry > 0 {
		head.Next = &ListNode{
			Val: carry,
		}
	}

	return dummyNode.Next
}

```

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *dummyNode = new ListNode(0);
        ListNode *cur = dummyNode;

        int val = 0, carry = 0;
        while(l1 || l2)
        {
            val = 0;
            if (l1)
            {
                val += l1->val;
                l1 = l1->next;
            }

            if (l2)
            {
                val += l2->val;
                l2 = l2->next;
            }

            val += carry;
            carry = val / 10;
            val = val % 10;
            cur->next = new ListNode(val);
            cur = cur->next;
        }

        //Tips(细节):最后可能多出来1位
        if (carry != 0)
            cur->next = new ListNode(carry);

        return dummyNode->next;
    }
};
```

## 方法二：递归

思路：递归将链表中的两数相加合并为6种如下情况，代码中有注释
1. 若l1, l2 都为空，且进位为0，直接返回nil
2. 若l1为空, l2 不空，且进位为0，直接返回l2
3. 若l2为空, l1 不空，且进位为0，直接返回l1
4. 若l1为空, l2 不空，且进位不为0，需要不断计算l2与进位
5. 若l1不空, l2 空，且进位不为0，需要不断计算l1与进位
6. 若l1不空, l2 不空，需要不断计算l1+l2+进位

```go
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
	if l1 == nil {
		return l2
	}

	if l2 == nil {
		return l1
	}

	return addTwoNumbersCore(l1, l2, 0)
}

func addTwoNumbersCore(l1 *ListNode, l2 *ListNode, carry int) *ListNode {

	//如果最后两个链表都空，但是还有进位
	if l1 == nil && l2 == nil && carry != 0 {
		return &ListNode{Val: carry}
	}else if l1 == nil && carry == 0 {  //如果l2不空，且没有进位
		return l2
	}else if l2 == nil && carry == 0 {	//如果l1不空，且没有进位
		return l1
	}else if l1 == nil && carry != 0 {	//如果l2不空，且有进位
		l2.Val = l2.Val + carry
		l2.Val, l2.Next = l2.Val % 10, addTwoNumbersCore(l1, l2.Next, l2.Val / 10)
		return l2
	} else if l2 == nil && carry != 0 {	//如果l1不空，且有进位
		l1.Val = l1.Val + carry
		l1.Val, l1.Next = l1.Val % 10, addTwoNumbersCore(l1.Next, l2, l1.Val / 10)
		return l1
	}


	//到这里说明l1, l2都有
	l1.Val, l1.Next = (l2.Val + l1.Val + carry) % 10, addTwoNumbersCore(l1.Next, l2.Next, (l2.Val + l1.Val + carry) / 10)

	return l1
}

```

**2021-01-20 递归的另外一种写法**
!> **着重在两个注释中的注意**
```go
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
	return addTwoNumbersCore(l1, l2, 0)
}

func addTwoNumbersCore(l1 *ListNode, l2 *ListNode, carry int) *ListNode {
    //注意1：如果两个都为空的时候不加这个条件将会执行if l1 == nil{}然后报错
    if l1 == nil && l2 == nil {
		//判断一下此时carry是否为0
		if carry != 0 {
			return &ListNode{
				Val:  carry,  //注意2：这里可能两个链表都遍历完成，但是carry不为0
			}
		}
		return nil
	}
	if l1 == nil {
		return &ListNode{
			Val:  (carry + l2.Val) % 10,
			Next: addTwoNumbersCore(nil, l2.Next, (carry+l2.Val)/10),
		}
	}
	if l2 == nil {
		return &ListNode{
			Val:  (carry + l1.Val) % 10,
			Next: addTwoNumbersCore(l1.Next, nil, (carry+l1.Val)/10),
		}
	}

	return &ListNode{
		Val:  (carry + l1.Val + l2.Val) % 10,
		Next: addTwoNumbersCore(l1.Next, l2.Next, (carry+l1.Val+l2.Val)/10),
	}
}
```
