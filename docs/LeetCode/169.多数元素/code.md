# [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

## 方法一：暴力美学进行统计

思路：使用map统计次数，之后每次统计的时候计算是否超过了一半数组长度，


时间复杂度：O(N)
空间复杂度：O(N)


```go
func majorityElement(nums []int) int {
	//如果只有1个元素
	if len(nums) == 1 {
		return nums[0]
	}
	mymap := make(map[int]int)
	for _, num := range nums {
		if _, ok := mymap[num]; ok {
			mymap[num] += 1
			if float64(mymap[num])/float64(len(nums)) >= 0.5 {
				return num
			}
		} else {
			mymap[num] = 1
		}

	}
	//说明啥都没有,到时候自己问面试官返回啥
	return 0
}
```

## 方法二：参考LeetCode-CookBook对方法一改进



时间复杂度：O(N)
		空间复杂度：O(N)


>执行用时：24 ms, 在所有 Go 提交中击败了65.56%的用户
> 		内存消耗：6.2    MB, 在所有 Go 提交中击败了5.48%的用户

```go
func majorityElement(nums []int) int {
	mymap := make(map[int]int)

	for _, num := range nums {
		mymap[num] += 1
		//注意这里是大于没有等于，因为len(nums)/2就会向下取整，题目中说出现次数大于 ⌊ n/2 ⌋ 的元素
		if mymap[num] > len(nums)/2 {
			return num
		}
	}

	return 0
}

```

## 方法三：使用额外空间的摩尔投票法

> 执行用时：20 ms, 在所有 Go 提交中击败了92.69%的用户
> 		内存消耗：6.6 MB, 在所有 Go 提交中击败了5.48%的用户

时间复杂度：O(N)
		空间复杂度：O(N)


```go
func majorityElement(nums []int) int {
	if len(nums) == 1 {
		return nums[0]
	}

	ret := []int{}
	for _, num := range nums {
		if len(ret) > 0 && num == ret[0] {
			ret = append(ret, num)
		} else if len(ret) > 0 && num != ret[0] {
			ret = ret[1:]
		} else {
			ret = append(ret, num)
		}
	}

	return ret[0]
}
```



## 【推荐】方法四：进阶-不使用额外空间的摩尔投票法

思路：

1. 从上面的方法二中其实观察出，我们不断地抵消数字，当次数为0就加入一个新数，所以只要一个数即可，
2. 但是我们需要保存次数，判断下次抵消后的次数或者新加入后的次数或者重置，我们的ret只需要保存一个数即可

> 执行用时：20 ms, 在所有 Go 提交中击败了92.69%的用户
> 		内存消耗：6.6 MB, 在所有 Go 提交中击败了5.48%的用户

时间复杂度：O(N)
		空间复杂度：O(1)


```go
func majorityElement(nums []int) int {
	//ret存储元素下标，count存储次数
	ret, count := 0, 1
	for i := 1; i < len(nums); i++ {
		if count > 0 && nums[ret] != nums[i] { //出现次数很多，但是这次我们不同于之前的数字，要抵消
			count -= 1
		} else if count > 0 && nums[ret] == nums[i] { //出现次数很多，这次又出现
			count += 1
		} else if count == 0 { //如果之前全部都抵消完
			count = 1
			ret = i
		}

		fmt.Println(i, nums[i])
	}

	return nums[ret]
}
```

## 总结：

关于摩尔投票法的理解：人数对打，如果1个人可以kill掉1个人，那么就是对打，最终的结果肯定是人数多的获胜，对拼消耗就是摩尔投票法的核心。

[摩尔投票法的理解](https://www.zhihu.com/question/49973163)

