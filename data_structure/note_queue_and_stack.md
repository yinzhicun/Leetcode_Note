<!--
 * @Author: yinzhicun
 * @Date: 2021-04-14 09:56:51
 * @LastEditTime: 2021-04-18 10:22:53
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \Leetcode_Note\data_structure\note_queue_and_stack.md
-->
# <center>队列与栈习题</center>

## 一、概念理解加深
STL中queue和stack的底层实现可以指定，默认为dequeue模板
### 1. 用栈实现队列
![](./picture/232.png)
> 1. 构建两个栈的数据结构stack_in, stack_out
> 2. 当有元素要进队时，stack_in进栈
> 3. 当有元素要出队时，stack_in元素全部出栈，顺序压入stack_out中，stack_out头元素出栈
> 4. 当两栈均为空时，即队列为空

```cpp
class MyQueue {
public:
    stack<int> stack_in;
    stack<int> stack_out; 
    /** Initialize your data structure here. */
    MyQueue() {}
    
    /** Push element x to the back of queue. */
    void push(int x) 
    {
        stack_in.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() 
    {
        if (stack_out.empty())
        {
            while (!stack_in.empty())
            {
                //STL库里stack容器的返回值为void
                int temp = stack_in.top();
                stack_in.pop();
                stack_out.push(temp);
            }
        }
        int result = stack_out.top();
        stack_out.pop();
        return result;
    }
    
    /** Get the front element. */
    int peek() 
    {
        int result = this->pop();
        stack_out.push(result);
        return result;
    }
    
    /** Returns whether the queue is empty. */
    bool empty() 
    {
        if (stack_out.empty() && stack_in.empty())
            return true;
        return false;
    }
};
```

### 2. 用队列实现栈
![](./picture/225.png)
> 1. 构建两个队列的数据结构que_in, que_backup
> 2. 当有元素要进栈时，que_in进队
> 3. 当有元素要出栈时，que_in除队尾元素元素在que_backup中备份，que_in元素全部出队，返回队尾元素
> 4. 当que_in为空时，即队列为空

```cpp
class MyStack {
public:
    queue<int> que_in;
    queue<int> que_backup;
    /** Initialize your data structure here. */
    MyStack() {}
    
    /** Push element x onto stack. */
    void push(int x) 
    {
        que_in.push(x);
    }
    
    /** Removes the element on top of the stack and returns that element. */
    int pop() 
    {
        int size = que_in.size();
        //留下队尾元素
        size--;
        //备份
        while (size--)
        {
            que_backup.push(que_in.front());
            que_in.pop();
        }
        int result = que_in.front();
        que_in.pop();
        //备份数据拷回
        que_in = que_backup;
        //清空备份队列
        while (!que_backup.empty())
            que_backup.pop();
        return result;
    }
    
    /** Get the top element. */
    int top() 
    {
        return que_in.back();
    }
    
    /** Returns whether the stack is empty. */
    bool empty() 
    {
        return que_in.empty();
    }
};
```

## 二、栈
### 1. 用栈解决匹配问题
#### 1.1 括号匹配
![](./picture/20.png)
- 时间复杂度为 **O(n)** ，空间复杂度为 **O(n+m)**
> 1. 明确一起有三种不匹配的情况，左右括号多余或者括号不匹配
> 2. 当出现左括号时右括号压栈，当出现匹配的右括号时出栈

```cpp
class Solution {
public:
    bool isValid(string s) 
    {
        stack<char> string_stack;
        int flag[3] = {0};
        for (auto& c : s)
        {
            if (c == '(')
                string_stack.push(')');
            else if (c == '[')
                string_stack.push(']');
            else if (c == '{')
                string_stack.push('}');
            else if (!string_stack.empty() && c == string_stack.top())    
                string_stack.pop();
            else
                return false;
        }
        return string_stack.empty();
    }
};
```

#### 1.2 字符匹配
![](./picture/1047.png)
- 时间复杂度为 **O(n)** ，空间复杂度为 **O(n)**
> 相同时弹栈，不同时压栈

```cpp
class Solution {
public:
    string removeDuplicates(string S) 
    {
        string result;
        for (auto& c : S)
        {
            if (result.size() !=0 && result.back() == c)
                result.pop_back();
            else
                result.push_back(c);
        }

        return result;
    }
};
```

#### 1.3 表达式计算
![](./picture/150.png)
- 时间复杂度为 **O(n)** ，空间复杂度为 **O(n)**
> 理解逆波兰表达式的意义，逆波兰表达式，实际上就是表达式树的后序遍历序列
![](./picture/150-1.png)
```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) 
    {
        stack<int> stk; 
        for (auto& c : tokens)
        {
            int a, b;
            if (!stk.empty() && c == "+")
            {
                b = stk.top();
                stk.pop();
                a = stk.top();
                stk.pop();
                int c = a + b;
                stk.push(c);
            }
            else if (!stk.empty() && c == "-")
            {
                b = stk.top();
                stk.pop();
                a = stk.top();
                stk.pop();
                int c = a - b;
                stk.push(c);
            }
            else if (!stk.empty() && c == "*")
            {
                b = stk.top();
                stk.pop();
                a = stk.top();
                stk.pop();
                int c = a * b;
                stk.push(c);
            }
            else if (!stk.empty() && c == "/")
            {
                b = stk.top();
                stk.pop();
                a = stk.top();
                stk.pop();
                int c = a / b;
                stk.push(c);
            }
            else
            {
                stk.push(stoi(c));
            }            
        }
        return stk.top();
    }
};
```

## 三、队列
### 1. 单调队列
![](./picture/239.png)
- 时间复杂度为 **O(n)** ，空间复杂度为 **O(k)**
> 1. 首先构造一个单调递减队列mon_que，该队列主要维护本题滑动窗口中的最大值，依据元素大小进行出队入队
> 2. 当需要压入元素a时，把右端所有比a小的元素全部弹出，保持队列元素的单调递减性
> 3. 当需要弹出元素时，若元素为最大值则弹出，反之不用进行任何操作
> 4. 滑动窗口时，弹出旧元素，压入新元素即可

```cpp
class Solution {
public:
    class mon_que{
    public:
        void push(int val)
        {
            while (!que.empty() && val > que.back())
                que.pop_back();
            que.push_back(val);
        }

        void pop(int val)
        {
            if (!que.empty() && val == que.front())
                que.pop_front();
        }

        int front()
        {
            return que.front();
        }
    
    private:
        deque<int> que; 
    };

    vector<int> maxSlidingWindow(vector<int>& nums, int k) 
    {
        vector<int> result;
        mon_que mque;
        for (int i = 0; i < k; i++)
            mque.push(nums[i]);

        result.push_back(mque.front());
        for (int i = k; i < nums.size(); i++)
        {
            mque.pop(nums[i - k]);
            mque.push(nums[i]);
            result.push_back(mque.front());
        }
        return result;
    }
};
```

### 2. 优先级队列
![](./picture/347.png)
- 时间复杂度为 **O(nlogn)** ，空间复杂度为 **O(n)**
> 1. 构造优先列就可以很容易的解决问题
> 2. 关键在于对priority_queue参数的使用与理解

```cpp
class Solution {
public:
    class pair_compair{
    public:
        bool operator()(const pair<int, int> a, const pair<int, int> b)
        {
            return a.second > b.second;
        }
    };
    
    vector<int> topKFrequent(vector<int>& nums, int k) 
    {
        unordered_map<int, int> map;
        vector<int> result;
        for (int i = 0; i < nums.size(); i++)
        {
            map[nums[i]]++;
        }

        priority_queue<pair<int, int>, vector<pair<int, int>>, pair_compair> pri_que;
        for (auto iter = map.begin(); iter != map.end(); iter++)
        {
            pri_que.push(*iter);
            if (pri_que.size() > k)
                pri_que.pop();
        }

        for (int i = 0; i < k ; i++)
        {
            result.push_back(pri_que.top().first);
            pri_que.pop();
        }
        return result;
    }
};
```