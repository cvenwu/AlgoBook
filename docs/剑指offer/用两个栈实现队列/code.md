# 用两个栈实现队列

**思路：**
- 准备1个输入数据的栈，1个输出数据的栈
- 当加入元素的时候直接加入到输入数据栈中
- 当弹出元素的时候，分为如下几种情况，
    1.  如果输出数据栈不为空，直接弹出
    2.  如果输出数据栈为空，将**输入数据栈所有元素**加入到输出数据栈
        1.  如果输出栈不为空，直接弹出并返回栈顶
        2.  否则返回-1

## 方法一：使用container/list

```go
type CQueue struct {
	InputStack, OutputStack *list.List
}


func Constructor() CQueue {
	return CQueue{list.New(), list.New()}
}


func (this *CQueue) AppendTail(value int)  {
	//直接将元素加入到输入栈中
	this.InputStack.PushBack(value)
}


func (this *CQueue) DeleteHead() int {
	//如果输出栈有元素，直接删除
	//否则需要将输入栈全部元素导入到输出栈，然后删除输出栈的元素
	if this.OutputStack.Len() == 0 {
		//以下3行代码必须这样写
		//或者可以写成this.OutputStack.PushBack(this.InputStack.Remove(this.InputStack.Back()))
        for this.InputStack.Len() > 0 {
			this.OutputStack.PushBack(this.InputStack.Remove(this.InputStack.Back()))
        }
    }
	if this.OutputStack.Len() != 0 {
		return this.OutputStack.Remove(this.OutputStack.Back()).(int)
    }
	return -1  //说明输入栈以及输出栈没有任何内容
}
```



## 方法二：使用切片
!> 注意：题目中有说明，如果队列为空且要弹出元素的时候直接返回-1

```go
type CQueue struct {
	inputStack  []int
	outputStack []int
}

func Constructor() CQueue {
	return CQueue{
		inputStack:  []int{},
		outputStack: []int{},
	}
}

func (this *CQueue) AppendTail(value int) {
	this.inputStack = append(this.inputStack, value)
}

func (this *CQueue) DeleteHead() int {
	//如果输出栈没有元素
	if len(this.outputStack) == 0 {
		//将输入栈的所有元素全部加入
		this.outputStack = append(this.outputStack, this.inputStack...)
		//输入栈将所有元素弹出
		this.inputStack = this.inputStack[0:0]
	}

	//此时如果输出栈有元素就弹出，否则返回-1
	if len(this.outputStack) != 0 {
		val := this.outputStack[0]
		this.outputStack = this.outputStack[1:]
		return val
	}

	return -1
}

/**
 * Your CQueue object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AppendTail(value);
 * param_2 := obj.DeleteHead();
 */
```

```cpp

class CQueue {
public:
    stack<int> inputStack;
    stack<int> outputStack;
    CQueue() {

    }
    
    void appendTail(int value) {
        this->inputStack.push(value);
    }
    
    int deleteHead() {
        //如果输出栈为空将所有输入栈的元素全部加入到输出栈里面
        if (this->outputStack.empty())
        {
            while(!this->inputStack.empty())
            {
                int topEle = this->inputStack.top();
                this->inputStack.pop();
                this->outputStack.push(topEle);
            }
        }

        //如果从输入栈中加入所有元素之后输出栈依然为空
        if (this->outputStack.empty())
            return -1;

         //从输出栈弹出一个栈顶并作为一个结果进行返回
        int ret = this->outputStack.top();
        this->outputStack.pop();
        return ret;
    }
};

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue* obj = new CQueue();
 * obj->appendTail(value);
 * int param_2 = obj->deleteHead();
 */
```