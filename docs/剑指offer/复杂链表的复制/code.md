# [剑指 Offer 35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

## 方法一：暴力

时间复杂度为O(N^2)

空间复杂度O(1)

## 方法二：使用一个哈希表映射<原节点，新复制的节点>

时间复杂度为O(N)

空间复杂度O(N)

## 方法三【推荐】：时间复杂度为O(N)空间复杂度O(1)

​	步骤：
 	1.  将每个节点复制到后面，同时random指向之前源指针的指向
	2.  修正每个新添加节点的random指向。注意：最后两个节点处理要特殊
	3.  将两个链表提取出来，注意：最后两个节点处理要特殊

>  执行用时：0 ms, 在所有 Go 提交中击败了 100.00% 的用户
> 		内存消耗：3.4 MB, 在所有 Go 提交中击败了 40.65% 的用户

```go

type Node struct {
	Val    int
	Next   *Node
	Random *Node
}


func copyRandomList(head *Node) *Node {
	//如果链表为空
	if head == nil {
		return nil
	}

	//第1步：在每个节点后边建立一个节点，并连接起来
	cur := head
	for cur != nil {
		cur.Next = &Node{
			Val:    cur.Val,
			Next:   cur.Next,
			Random: cur.Random,
		}
		cur = cur.Next.Next
	}

	//第2步：将新建立的每个节点的Random指针指向正确方向
	cur = head.Next
	for cur != nil {
		if cur.Random != nil {
			cur.Random = cur.Random.Next
		}

		//如果是最后一个节点
		if cur.Next != nil {
			cur = cur.Next.Next
		} else {
			break
		}
	}

	//第3步：将奇数节点与偶数节点对应的两个链表分开
	cur = head.Next
	odd, even := head, head.Next
	for odd != nil {
		if even.Next != nil {
			odd.Next, even.Next = odd.Next.Next, even.Next.Next
			odd, even = odd.Next, even.Next
		} else { 		//如果最后两个节点
			odd.Next = odd.Next.Next
			break
		}
	}

	return cur
}

```

自己过一段时间的另外一种写法：
```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Next *Node
 *     Random *Node
 * }
 */

func copyRandomList(head *Node) *Node {
    //如果当前节点为空，直接返回
    if head == nil {
        return nil
    }
    tempHead := copyNodeToNewLinkedList(head)

    return splitLinkedList(tempHead)
}

//将原链表的节点复制到后面生成一个新链表
func copyNodeToNewLinkedList(head *Node) *Node {
    //首先将每个节点后面复制一个同样的节点
    cur := head
    for cur != nil {
        node := &Node{
            Val: cur.Val,
            Next: cur.Next,
        }
        cur.Next, cur = node, cur.Next
    }

    //将新复制出来的每个节点的random正确指向
    cur = head
    for cur != nil {
        if cur.Random != nil {
            cur.Next.Random = cur.Random.Next
        }
        cur = cur.Next.Next
    }
    return head
}


//将我们自己新复制出来的节点拆分成一个链表并返回头
func splitLinkedList(head *Node) *Node {
    cur := head
    ret := head.Next

    for cur != nil && cur.Next != nil{
        cur.Next, cur = cur.Next.Next, cur.Next 
    }

    return ret
}
```

## 【推荐】方法四：方法三的改进

**2021-03-06**

注意：原链表中有的节点的random会指向nil，所以要if判断

```go

func copyRandomList(head *Node) *Node {
	if head == nil {
		return nil
	}

	head = copyLinkedList(head)
	anotherHead := splitLinkedList(head)
	return anotherHead
}

func splitLinkedList(head *Node) *Node {
	cur := head
	anotherHead := cur.Next

	for cur != nil && cur.Next != nil {
		cur.Next, cur = cur.Next.Next, cur.Next
	}
	return anotherHead
}

func copyLinkedList(head *Node) *Node {
	cur := head
	for cur != nil {
		node := &Node{
			Val: cur.Val,
			Next: cur.Next,
		}
		cur.Next, cur = node, cur.Next
	}

    cur = head
    for cur != nil {
        // fmt.Println(cur.Val)
        cur = cur.Next
    }

	//再给random赋值
	cur = head
	for cur != nil {
        if cur.Random != nil {
            cur.Next.Random = cur.Random.Next
        }
        //注意：因为可能有的random会指向nil
        cur = cur.Next.Next
	}

	return head
}

```