# [208. 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

## 方法一：参考Leetcode-cookbook

思路：通过系统提供的map建立映射


> 执行用时：88ms, 在所有 Go 提交中击败了23.01%的用户
> 		内存消耗：16.6 MB, 在所有 Go 提交中击败了55.39%的用户


```go

type Trie struct {
	isWord bool
	children map[rune]*Trie
}


/** Initialize your data structure here. */
func Constructor() Trie {
	return Trie{
		isWord: false,
		children: make(map[rune]*Trie),
	}
}


/** Inserts a word into the trie. */
func (this *Trie) Insert(word string)  {
	parent := this
	//遍历每个字符
	for _, ch :=  range word {
		if v, ok := parent.children[ch]; ok {
			parent = v
		} else {
			parent.children[ch] = &Trie{
				isWord: false,
				children: make(map[rune]*Trie),
			}
			parent = parent.children[ch]
		}
	}
	parent.isWord = true
}


/** Returns if the word is in the trie. */
func (this *Trie) Search(word string) bool {
	parent := this

	//遍历每个字符
	for _, ch :=  range word {
		if v, ok := parent.children[ch]; ok {
			parent = v
		} else {
			return false
		}
	}

	//走到最后可能不是一个单词，因为可能是一个前缀
	return parent.isWord
}


/** Returns if there is any word in the trie that starts with the given prefix. */
func (this *Trie) StartsWith(prefix string) bool {
	parent := this
	//遍历每个字符
	for _, ch :=  range prefix {
		if v, ok := parent.children[ch]; ok {
			parent = v
		} else {
			return false
		}
	}

	return true
}


/**
 * Your Trie object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Insert(word);
 * param_2 := obj.Search(word);
 * param_3 := obj.StartsWith(prefix);
 */

```

## 方法二：

思路：通过一个长度为26的切片进行hash映射

> 执行用时：52 ms, 在所有 Go 提交中击败了97.01%的用户
> 		内存消耗：18.3 MB, 在所有 Go 提交中击败了46.39%的用户

```go
type Trie struct {
	endOfWord bool
	children [26]*Trie
}


/** Initialize your data structure here. */
func Constructor() Trie {
	return Trie{}  //这里系统会帮助我们提前创建好endOfWord以及children
}


/** Inserts a word into the trie. */
func (this *Trie) Insert(word string)  {
	cur := this
	for _, val := range word {
		if cur.children[val-'a'] != nil {
			cur = cur.children[val-'a']
		} else {
			cur.children[val-'a'] = &Trie{}
			cur = cur.children[val-'a']
		}
	}

	cur.endOfWord = true
}


/** Returns if the word is in the trie. */
func (this *Trie) Search(word string) bool {
	cur := this
	for _, val := range word {
		if cur.children[val-'a'] != nil {
			cur = cur.children[val-'a']
		} else {
			return false
		}
	}
	return cur.endOfWord
}


/** Returns if there is any word in the trie that starts with the given prefix. */
func (this *Trie) StartsWith(prefix string) bool {
	cur := this
	for _, val := range prefix {
		if cur.children[val-'a'] != nil {
			cur = cur.children[val-'a']
		} else {
			return false
		}
	}
	return true
}


/**
 * Your Trie object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Insert(word);
 * param_2 := obj.Search(word);
 * param_3 := obj.StartsWith(prefix);
 */
```

