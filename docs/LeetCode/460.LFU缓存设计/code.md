
# [460.LFU 缓存](https://leetcode-cn.com/problems/lfu-cache/)

## 方法一：



```go
type DoubleLinkedListNode struct {
	key, val, freq int
	prev, next     *DoubleLinkedListNode
}

type LFUCache struct {
	minFreq int
	size    int
	cap     int
	cache   map[int]*DoubleLinkedListNode
	freq    map[int]*DoubleLinkedList
}

type DoubleLinkedList struct {
	head, tail *DoubleLinkedListNode
}



func Constructor(capacity int) LFUCache {
	return LFUCache{
		minFreq: 0,
		cap:     capacity,
		size:    0,
		cache:   make(map[int]*DoubleLinkedListNode),
		freq:    make(map[int]*DoubleLinkedList),
	}
}

func (this *LFUCache) Get(key int) int {
	//注意点：
	if this.cap <= 0 {
		return -1
	}
	//判断key是否存在对应的node
	if node, ok := this.cache[key]; ok {
		this.addFreq(key)
		//返回node对应的值
		return node.val
	}

	//说明不存在
	return -1
}

func (this *LFUCache) Put(key int, value int) {
	//注意点：
	if this.cap <= 0 {
		return
	}

	//判断key是否存在对应的node
	if node, ok := this.cache[key]; ok {
		node.val = value
		this.addFreq(key)
		return
	}

	//走到这里，说明对应的key的节点不存在
	//1. 判断size是否等于cap
	if this.cap == this.size {
		//1. 从最小访问频次链表移除尾部节点
		node := this.freq[this.minFreq].tail.prev
		node.next.prev, node.prev.next = node.prev, node.next
		//2.删除在cache中的项
		delete(this.cache, node.key)
		//3.让当前大小减去1
		this.size--
	}

	//2. 新建节点
	node := &DoubleLinkedListNode{
		val: value,
		key: key,
		freq: 1,  //易错点，因为我们是先将其freq置为1，然后调用函数的时候会将其放置到freq对应的链表中
	}

	//3. 加入到cache中
	this.cache[key] = node

	//4. 加入到次数为1对应的链表头部
	this.addNode(key)

	//5. size++
	this.size++

	//6. 最小频次置1
	this.minFreq = 1
}

/*
	将node对应的次数加1,包含如下步骤：
	1. 将node从原来对应次数的链表中移除
	2. 将node加入到新次数(新次数为原次数+1)对应的链表的头部
	3. 将node.freq+1
	4. 判断最小频次对应的链表是否为空，如果为空，则最小频次+1
*/
func (this *LFUCache) addFreq(key int) {
	this.removeNode(key)
	this.cache[key].freq++
	this.addNode(key)
	if this.freq[this.minFreq].empty() {
		this.minFreq++
	}
}

//判断链表是否为空
func (d *DoubleLinkedList) empty() bool {
	return d.head.next == d.head.prev
}

//从次数链表中移除key对应的节点
func (this *LFUCache) removeNode(key int) {
	node := this.cache[key]
	node.prev.next, node.next.prev, node.next, node.prev = node.next, node.prev, node, node
}

//将key对应的节点加入到次数链表中
func (this *LFUCache) addNode(key int) {
	node := this.cache[key]

	//如果对应的次数的链表为空，则初始化
	if _, ok := this.freq[node.freq]; !ok {
		head := &DoubleLinkedListNode{}
		tail := &DoubleLinkedListNode{}
		head.next, head.prev, tail.next, tail.prev = tail, tail, head, head
		this.freq[node.freq] = &DoubleLinkedList{
			head: head,
			tail: tail,
		}
	}
	head := this.freq[node.freq].head
	head.next, head.next.prev, node.next, node.prev = node, node, head.next, head
}

/**
 * Your LFUCache object will be instantiated and called as such:
 * obj := Constructor(capacity);
 * param_1 := obj.Get(key);
 * obj.Put(key,value);
 */

func main() {
	lfu := Constructor(3)


	lfu.Put(2,2)
	lfu.Put(1,1)
	lfu.Get(2)
	lfu.Get(1)
	lfu.Get(2)
	lfu.Put(3,3)
	lfu.Put(4,4)
	lfu.Get(3)
	lfu.Get(2)
	lfu.Get(1)
	lfu.Get(4)
}

```