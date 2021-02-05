# [146. LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)

## 方法：采用map+双链表

为什么要采用map：因为get的时候就是查找，我们要O(1)时间复杂度

为什么采用双链表：因为我们将当前节点提升为最近使用时，要修改当前节点的上一个节点的next指向，所以我们需要采用双链表。

同时注意：我们在map中采用key-val存储，我们在链表中的节点也要存储key-val，因为我们删除节点的时候，查找到了当前节点，如果只存val，我们需要没办法用O(1)的时间找到key，从而就不能在map中用O(1)的操作删除

思路：

1. get操作：
   1. 如果节点存在，将节点提升为最近使用(首先删除当前节点，之后将当前节点移动到head的下一个节点)并返回节点的值
   2. 如果不存在直接返回-1
2. put操作：
   1. 首先查看map中是否存在该节点
      1. 如果存在
         1. 就修改该节点的值
         2. 删除该节点
         3. 将该节点提升为最近使用(放置到head的下面)
      2. 如果不存在
         1. 新建一个节点，插入到head后面
         2. 同时判断map容量是否满了，如果满了就要从链表尾部删除最近不经常使用的，**之后记得删除map中的键值对哦，这里需要借助节点存储的key删除，所以我们节点要存储key信息**

> 执行用时：116 ms, 在所有 Go 提交中击败了99.90%的用户
> 		内存消耗：12.6 ms, 在所有 Go 提交中击败了94.19%的用户

```go

/**
 * @Author: yirufeng
 * @Email: yirufeng@foxmail.com
 * @Date: 2020/9/5 8:50 下午
 * @Desc: 146.LRU缓存机制
 */

//定义双链表的节点
type DoubleLinkedListNode struct {
	Key, Val   int //定义Key也就是map的key，到时候根据链表中的节点的key就可以删除map对应的key，不然比较难删除
	Prev, Next *DoubleLinkedListNode
}

type LRUCache struct {
	dict       map[int]*DoubleLinkedListNode //键值对，键为，值指向双链表的一个节点
	head, tail *DoubleLinkedListNode         //双链表
	capacity   int
}

func Constructor(capacity int) LRUCache {
	tempHead := &DoubleLinkedListNode{

	}
	tempTail := &DoubleLinkedListNode{
		//下面注释的这两个指针其实用不上
		//Prev: tempHead,
		//Next: tempHead,
	}

	tempHead.Next, tempTail.Prev = tempTail, tempHead

	return LRUCache{
		dict:     make(map[int]*DoubleLinkedListNode),
		head:     tempHead,
		tail:     tempTail,
		capacity: capacity,
	}
}

//从删除当前节点
func (this *LRUCache) removeNode(cur *DoubleLinkedListNode) {
	cur.Prev.Next, cur.Next.Prev = cur.Next, cur.Prev
}

//将当前节点添加到head后面
func (this *LRUCache) addToHead(cur *DoubleLinkedListNode) {
	this.head.Next, this.head.Next.Prev, cur.Next, cur.Prev = cur, cur, this.head.Next, this.head
}

//将当前节点移动到头部
//1. 删除当前节点
//2. 添加到头部
func (this *LRUCache) moveToHead(cur *DoubleLinkedListNode) {
	this.removeNode(cur)
	this.addToHead(cur)
}

//从尾部删除节点
func (this *LRUCache) removeTail() *DoubleLinkedListNode {
	cur := this.tail.Prev
	this.removeNode(cur)
	return cur
}

func (this *LRUCache) Get(key int) int {
	if cur, ok := this.dict[key]; ok { //说明之前就存在，
		//1. 删除当前节点
		//2. 提升为最近使用，也就是插入到头部
		this.moveToHead(cur)
		return cur.Val
	}

	//说明不存在
	return -1
}

func (this *LRUCache) Put(key int, value int) {

	if cur, ok := this.dict[key]; ok { //不为-1，修改对应的值即可，	并且提升为最近使用
		//1. 从修改对应的值
		cur.Val = value
		//2. 提升为最近使用
		this.moveToHead(cur)
		//fmt.Println("修改对应的值并提升为最近使用---->", key, "-", value)
	} else { //如果不存在，插入并且提升为最近使用

		cur = &DoubleLinkedListNode{
			Val: value,
			Key: key,
		}
		this.addToHead(cur)
		this.dict[key] = cur
		//如果容量满
		if len(this.dict) > this.capacity {
			//1.删除尾部节点
			node := this.removeTail()
			//2.同时记得从map里面删除哦
			delete(this.dict, node.Key)
		}
		//fmt.Println("插入到head后面-------->", key, "-", value)
	}
	//fmt.Println("map元素个数：", len(this.dict))
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * obj := Constructor(capacity);
 * param_1 := obj.Get(key);
 * obj.Put(key,value);
 */
```


!> 2021-02-05更新代码如下：

```go
type LRUCache struct {
	cap int
	size int
	head *DLinkedNode
	tail *DLinkedNode
	hmap map[int]*DLinkedNode
}

type DLinkedNode struct {
	key, val int
	prev, next *DLinkedNode
}

func Constructor(capacity int) LRUCache {
	l := LRUCache{
		cap: capacity,
		size: 0,
		head: &DLinkedNode{
		},
		tail: &DLinkedNode{
		},
		hmap: make(map[int]*DLinkedNode),
	}
	
	l.head.prev, l.head.next, l.tail.prev, l.tail.next = l.tail, l.tail, l.head, l.head
	//l.head.next, l.tail.prev其实用不上，可以不写，等价于下面这句
	// l.head.prev, l.tail.next = l.tail,  l.head
	return l
}

//从双链表中移除节点
func (this *LRUCache) removeNode (node *DLinkedNode) {
	node.prev.next, node.next.prev = node.next, node.prev
}
 
//在头部添加节点
func (this *LRUCache) addToHead(node *DLinkedNode) {
	this.head.next, this.head.next.prev, node.prev, node.next = node, node, this.head, this.head.next
}

func (this *LRUCache) Get(key int) int {
	//如果存在
	if node, ok := this.hmap[key]; ok {
		//提升为最近使用
		this.moveToHead(node)
		//返回值
		return node.val
	}

	//说明不存在
	return -1
}

//将node从其他位置移动到头部,相当于提升为最近使用
func (this *LRUCache) moveToHead(node *DLinkedNode) {
	this.removeNode(node)
	this.addToHead(node)
} 


func (this *LRUCache) Put(key int, value int)  {
	//首先判断是否存在,如果不存在就新建并添加到头部
	if node, ok := this.hmap[key]; !ok {
		node = &DLinkedNode {
			key: key,
			val: value,
		}
		//加入到map中
		this.hmap[key]=node
		//加入到链表中
		this.addToHead(node)
		this.size++
		//如果长度超过了容量，移除最久未使用的
		if this.size > this.cap {
			//还得从map中删除
			delete(this.hmap, this.tail.prev.key)
			this.removeNode(this.tail.prev)
			this.size--
		}
	} else {
		//如果存在也可能更新值
		node.val = value
		//移动到头部
		this.moveToHead(node)
		
	}
}


/**
 * Your LRUCache object will be instantiated and called as such:
 * obj := Constructor(capacity);
 * param_1 := obj.Get(key);
 * obj.Put(key,value);
 */
```