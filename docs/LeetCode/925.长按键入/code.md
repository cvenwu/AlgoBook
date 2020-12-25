# [925. 长按键入](https://leetcode-cn.com/problems/long-pressed-name/)

## 方法一：


思路：双指针，分别指向传入两个字符串的第1个字符

步骤：
1. first, second分别指向两个字符串的第1个字符，
2. 开始进行循环：
   2.1 如果相等，first,second都后移1个单位
   2.2 如果不等，并且second不是第1个下标，此时second可能是前面一个字符重复加入，因此验证second的前一个字符与现在的指向字符是否相等
   2.3 否则返回false
3. 循环结束：我们需要处理几个剩下的条件，可能我们最后second没有指向最后并且second后面还有其他字符，因此我们验证此时second后面是否有其他不同的字符，如果有就返回false
4. 最后返回的时候我们需要处理是否first指向最后，因为可能second先于first指向最后

其实上面的思路就是用滑动窗口做这个题目，如果字符相等就往后移动，如果不等时，第2个字符串的字符与前一个字符相等，也会继续后移，直到遇到不同的字符就返回false

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.1 MB, 在所有 Go 提交中击败了13.48%的用户


```go
func isLongPressedName(name string, typed string) bool {
	first, second := 0, 0
	for first < len(name) && second < len(typed) {
		if name[first] == typed[second] {
			first += 1
			second += 1

			//如果两个不等，验证是否前面一个键盘字符有重复
		} else if name[first] != typed[second] && second >= 1 && typed[second-1] == typed[second] {
			second += 1
		} else {
			return false
		}
	}

	//注意点1：最后typed可能还有别的剩余的字符还是需要处理一下的
	for second < len(typed) {
		if typed[second-1] != typed[second] {
			return false
		}
		second += 1
	}

	//注意点2：这里需要验证fist是否走到了name的最后
	return first == len(name)
}
```

