# 滑动窗口的最大值

思路：使用一个切片模拟窗口以及一个切片保存滑动窗口每次的最大值

1. 初始时将前滑动窗口大小的元素中的元素按照如下规则加入最大值切片
   1. 若最大值切片没有元素，直接加入
   2. 若大于最大值切片的最后一个，则弹出最后一个元素，不断循环，直到最大值切片最后一个元素小于等于要加入的元素值
2. 之后在加入当前遍历到的元素时，需要确保如下几种情况
   1. 滑动窗口最左边的元素没有越界
   2. 加入的元素如果比之前的大，就弹出，直到最大值切片最后一个元素小于等于要加入的元素值
   3. 此时就可以直接加入





## **自己纸上手写的代码：(提交不通过)**

> *//语法有问题：第2个for循环的里面那个for循环，可能为0，因此就会把索引设为-1，报错*
>
> *//同时逻辑有点小问题，我们自己初始化了滑动窗口大小的元素，初始化完成之后，没有将初始化后的最大值加入到我们的结果中*

```go
func maxSlidingWindow(nums []int, k int) []int {
	//如果传入的数组大小小于窗口大小，我们需要问面试官如何处理
	if len(nums) < k {
		return nil
	}

	//window用来模拟窗口，ret用来存放结果
	window, ret, curLength := []int{}, []int{}, 0
	//处理前k个数(特殊情况)
	for i := 0; i < k; i++ {
		//再使用一层循环，因为可能加入的比前面几个元素都大，所以前面的元素全部都要弹出
		for curLength != 0 && nums[window[curLength - 1]] < nums[i] {
			window = window[:curLength - 1]
			curLength -= 1
		}
		window = append(window, i)  //注意window加入的元素是下标不是值
		curLength += 1
	}

	for i := k; i < len(nums); i++ {
		if (i - window[0]) >= k {  //说明左边的元素超过了边界要弹出
			window = window[1:]
			curLength -= 1
		}

		for curLength < k && nums[window[curLength - 1]] < nums[i] {
			//弹出窗口最右边的元素，说明加入的比已经在里面的最后一个元素大
			window = window[:curLength - 1]
			curLength -= 1
		}
		window = append(window, i)
		curLength += 1
		ret = append(ret, nums[window[0]])
	}

	return ret
}
```



## 对上面版本的代码的错误改正：

> *//提交之后报错：因为传入的数组以及滑动窗口大小可能为0*
>
> *// 加了`ret = append(ret, nums[window[0]])`，改了`for curLength != 0 && curLength < k && nums[window[curLength - 1]] < nums[i] {`*

```go
func maxSlidingWindow(nums []int, k int) []int {

	//如果传入的数组大小小于窗口大小，我们需要问面试官如何处理
	// if len(nums) < k {
	// 	return nil
	// }

	//window用来模拟窗口，ret用来存放结果
	window, ret, curLength := []int{}, []int{}, 0
	//处理前k个数(特殊情况)
	for i := 0; i < k; i++ {
		//再使用一层循环，因为可能加入的比前面几个元素都大，所以前面的元素全部都要弹出
		for curLength != 0 && nums[window[curLength - 1]] < nums[i] {
			window = window[:curLength - 1]
			curLength -= 1
		}
		window = append(window, i)  //注意window加入的元素是下标不是值
		curLength += 1
	}
	ret = append(ret, nums[window[0]])
	for i := k; i < len(nums); i++ {
		if (i - window[0]) >= k {  //说明左边的元素超过了边界要弹出
			window = window[1:]
			curLength -= 1
		}
		
		for curLength != 0 && curLength < k && nums[window[curLength - 1]] < nums[i] {
			//弹出窗口最右边的元素，说明加入的比已经在里面的最后一个元素大
			window = window[:curLength - 1]
			curLength -= 1
		}
		window = append(window, i)
		curLength += 1
		ret = append(ret, nums[window[0]])
	}

	return ret
}
```



## 再改正：

> *// 执行用时：20 ms, 在所有 Go 提交中击败了 79.30% 的用户*
>
> *// 内存消耗：6.3 MB, 在所有 Go 提交中击败了 41.67% 的用户*

```go
func maxSlidingWindow(nums []int, k int) []int {
	if len(nums) <= 0 || k <= 0 {
		return nil
	}

	//如果传入的数组大小小于窗口大小，我们需要问面试官如何处理
	// if len(nums) < k {
	// 	return nil
	// }

	//window用来模拟窗口，ret用来存放结果
	window, ret, curLength := []int{}, []int{}, 0
	//处理前k个数(特殊情况)
	for i := 0; i < k; i++ {
		//再使用一层循环，因为可能加入的比前面几个元素都大，所以前面的元素全部都要弹出
		for curLength != 0 && nums[window[curLength - 1]] < nums[i] {
			window = window[:curLength - 1]
			curLength -= 1
		}
		window = append(window, i)  //注意window加入的元素是下标不是值
		curLength += 1
	}
	ret = append(ret, nums[window[0]])
	for i := k; i < len(nums); i++ {
		if (i - window[0]) >= k {  //说明左边的元素超过了边界要弹出
			window = window[1:]
			curLength -= 1
		}
		
		for curLength != 0 && curLength < k && nums[window[curLength - 1]] < nums[i] {
			//弹出窗口最右边的元素，说明加入的比已经在里面的最后一个元素大
			window = window[:curLength - 1]
			curLength -= 1
		}
		window = append(window, i)
		curLength += 1
		ret = append(ret, nums[window[0]])
	}

	return ret
}
```

