<!--
 * @Author: yinzhicun
 * @Date: 2021-04-04 09:36:07
 * @LastEditTime: 2021-04-12 21:14:46
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /Leetcode_Note/data_structure/note_array.md
-->

# <center>数组习题</center>

## 一、一维数组&字符数组

### 1. 搜索
#### 1.1 二分排序
![avastar](./picture/35.png)
- 暴力法求解，时间复杂度为 **O(n)** ，空间复杂度为 **O(1)**

> 1. 循环搜索
> 2. 搜索的同时比较大小判定target在数组中的位置

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) 
    {
        //记录target在数组中的位置
        int target_index = 0;
        for (int i = 0; i < nums.size(); i++)
        {
            if (nums[i] == target)
                return i;
            else
            {
                if (nums[i] < target)
                    target_index ++;
            }
        }
        return target_index;
    }
};
```

- 二分法求解，时间复杂度为 **O(log n)**，空间复杂度为 **O(1)**
> 二分查找，关键在于抓住循环不变量，一法为左闭又开区间，二法为闭区间

```cpp
//法1
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) 
    {
        //初始化，设定区间为左闭右开区间,此为循环不变量
        int left = 0;
        int right = nums.size();
        int middle;
        while (left < right)
        {
            middle = (left + right) / 2;
            if (nums[middle] == target)
                return middle;
            else if (nums[middle] < target)
                left = middle + 1;
            else
                right = middle;
        }
        // 分别处理如下四种情况
        // 目标值在数组所有元素之前 [0,0)
        // 目标值等于数组中某一个元素 return middle
        // 目标值插入数组中的位置 [left, right) ，returnright 即可
        // 目标值在数组所有元素之后的情况 [left, right)returnright 即可
        return right;
    }
};

//法2
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) 
    {
        //初始化，设定区间为闭区间,此为循环不变量
        int left = 0;
        int right = nums.size() - 1;
        int middle;
        while (left <= right)
        {
            middle = (left + right) / 2;
            if (nums[middle] == target)
                return middle;
            else if (nums[middle] < target)
                left = middle + 1;
            else
                right = middle - 1;
        }
        return left;
    }
};
```

### 2. 数组里的双指针法
#### 2.1 删除元素
![avastar](./picture/27.png)
- 暴力法求解，时间复杂度为 **O(n^2)** ，空间复杂度为 **O(1)**
> 1. 循环搜索
> 2. 每当出现需要删除元素时，将其删除，并将数组后面部分前移一位
> 3. 由于数组元素移动，指针不变，在同一个位置比较新改动的元素后再继续移动

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) 
    {
        int size = nums.size();
        for (int i = 0; i < size; i++)
        {
            if (nums[i] == val)
            {
                for (int j = i; j < size - 1; j++)
                {
                    nums[j] = nums[j + 1];
                }
                size--;
                //因为移动了一个元素，所以要回退一个进行比较
                i--;
            }
        }
        return size;
    }
};
```

- 双指针法求解，时间复杂度为 **O(n)**，空间复杂度为 **O(1)**
> 1. 快指针与慢指针一起移动
> 2. 当快慢指针遇到指定删除元素时，慢指针不动，快指针移动，直到越过需要删除元素
> 3. 从快指针指向的第一个元素开始，其移到慢指针所指之处

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) 
    {
        int slow_index = 0;
        int fast_index = 0;
        for (; fast_index < nums.size(); fast_index++)
        {   
            if (nums[fast_index] != val)
            {
                nums[slow_index] = nums[fast_index];
                slow_index++;
            }
        }
        return slow_index;
    }
};
```

#### 2.2 滑动窗口
![avastar](./picture/209.png)
- 暴力法求解，时间复杂度为 **O(n^2)** ，空间复杂度为 **O(1)**
> 1. 循环搜索
> 2. 每一次大循环中对后续子序列进行顺序加和比较
> 3. 当满足大于等于target的条件后，取最小长度

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) 
    {
        int result = INT32_MAX; // 最终的结果
        int sum = 0; // 子序列的数值之和
        int subLength = 0; // 子序列的长度
        for (int i = 0; i < nums.size(); i++) 
        { // 设置子序列起点为i
            sum = 0;
            for (int j = i; j < nums.size(); j++) 
            { // 设置子序列终止位置为j
                sum += nums[j];
                if (sum >= target) 
                { // 一旦发现子序列和超过了s，更新result
                    subLength = j - i + 1; // 取子序列的长度
                    result = result < subLength ? result : subLength;
                    break; // 因为我们是找符合条件最短的子序列，所以一旦符合条件就break
                }
            }
        }
        // 如果result没有被赋值的话，就返回0，说明没有符合条件的子序列
        return result == INT32_MAX ? 0 : result; 
    }
};
```

- 双指针法求解，时间复杂度为 **O(nlogn)**，空间复杂度为 **O(1)**
> 1. 慢指针先不动，快指针移动
> 2. 窗口元素之和（即快慢指针之间的元素和）大于等于target时，快指针不动，慢指针迁移，实时记录长度
> 3. 当到不满足条件后，快指针继续移动
> 4. 实际运用了动态规划的思想

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) 
    {
        int slow_index = 0;
        int fast_index = 0;
        int sum = 0;
        int length = INT_MAX;
        for ( ; fast_index < nums.size(); fast_index++)
        {   
            sum = sum + nums[fast_index];
            while (sum >= target)
            {
                sum = sum - nums[slow_index];
                slow_index++;
                length = length > (fast_index - slow_index + 2) ? (fast_index - slow_index + 2) : length;
            }
        }
        return length == INT_MAX ? 0 : length;
    }
};
```

#### 2.3 几数之和
##### 2.3.1 三数之和
![avastar](./picture/15.png)
- 时间复杂度为 **O(n^2)**，空间复杂度为 **O(logn)**
> 1. 对nums先排序，然后依次遍历
> 2. 遍历的同时使用双指针在i的右半部分从两端向中间靠拢，不断比较nums[i]+nums[left]+nums[right]与0的值
> 3. 两次去重，第一次在外层for循环中；第二次是在每一次找到一个三元组之后

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) 
    {
        vector<vector<int>> result;
        if (nums.size() < 3)
            return result;
        sort(nums.begin(), nums.end());
        //第一个指针，确定第一个元素
        for (int first_index = 0; first_index < nums.size() - 2; first_index++)
        {   
            //去初始元素的重
            //使用first与first - 1防止漏掉两个数一样的情况
            if (first_index > 0 && nums[first_index] == nums[first_index - 1])
            {
                continue;
            }
            //第二个指针，正方向移动
            int second_index = first_index + 1;
            //第三个指针，反方向移动
            int third_index = nums.size() - 1;
            while (second_index < third_index)
            {
                if (nums[first_index] + nums[second_index] + nums[third_index] > 0)
                    third_index--;
                else if (nums[first_index] + nums[second_index] + nums[third_index] < 0)
                    second_index++;
                else
                {
                    result.push_back({nums[first_index], 
                                      nums[second_index], 
                                      nums[third_index]});
                    //中间元素去重，在找到一组值之后去重，较为合理
                    while (nums[third_index] == nums[third_index - 1] && second_index < third_index)
                        third_index--;
                    while (nums[second_index] == nums[second_index + 1] && second_index < third_index)
                        second_index++;

                    second_index++;
                    third_index--;
                }
            }
            
        }
        return result;
    }
};
```

##### 2.3.2 四数之和
![avastar](./picture/18.png)
- 时间复杂度为 **O(n^3)**，空间复杂度为 **O(logn)**
> 1. 对nums先排序，然后依次遍历，实际上就是在三数之和的基础上稍加改进
> 2. 遍历的同时使用双指针在i的右半部分从两端向中间靠拢，不断比较nums[i]+nums[left]+nums[right]与t]rget - nums[i]的值
> 3. 三次去重，第一次在外层for循环中；第二次在第二层for循环中；第三次是在每一次找到一个三元组之后

```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) 
    {
        vector<vector<int>> result;
        if (nums.size() < 4)
            return result;
        sort(nums.begin(), nums.end());
        //i为外指针，将四数之和问题转化为三数求解
        for (int i = 0; i < nums.size() - 3; i++)
        {   
            if (i > 0 && nums[i] == nums[i - 1])
                continue;
            //第一个指针，确定第一个元素
            for (int first_index = i + 1; first_index < nums.size() - 2; first_index++)
            {   
                //去初始元素的重
                //使用first与first - 1防止漏掉两个数一样的情况
                if (first_index > i + 1 && nums[first_index] == nums[first_index - 1])
                {
                    continue;
                }
                //第二个指针，正方向移动
                int second_index = first_index + 1;
                //第三个指针，反方向移动
                int third_index = nums.size() - 1;
                while (second_index < third_index)
                {
                    if (nums[first_index] + nums[second_index] + nums[third_index] > target - nums[i])
                        third_index--;
                    else if (nums[first_index] + nums[second_index] + nums[third_index] < target - nums[i])
                        second_index++;
                    else
                    {
                        result.push_back({nums[i],
                                          nums[first_index], 
                                          nums[second_index], 
                                          nums[third_index]});
                        //中间元素去重，在找到一组值之后去重，较为合理
                        while (nums[third_index] == nums[third_index - 1] && second_index < third_index)
                            third_index--;
                        while (nums[second_index] == nums[second_index + 1] && second_index < third_index)
                            second_index++;
    
                        second_index++;
                        third_index--;
                    }
                }
            }
        }
       
        return result;
    }
};
```

#### 2.4 反转字符串
##### 2.4.1 普通反转
![avastar](./picture/344.png)
- 双指针法求解，时间复杂度为 **O(n)**，空间复杂度为 **O(1)**
> 头指针与尾指针交换元素同时像中间移动，反转实际上可以看成一种对成变换

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) 
    {   
        int start_index = 0;
        int end_index = s.size() - 1;
        while (start_index <= end_index)
        {
            char temp = s[start_index];
            s[start_index] = s[end_index];
            s[end_index] = temp;
            start_index++;
            end_index--;
        }
    }
};
```

##### 2.4.2 特殊条件反转
![avastar](./picture/541.png)
- 在上题的基础上加条件，时间复杂度为 **O(n)**，空间复杂度为 **O(1)**
> 头指针与尾指针交换元素同时像中间移动，反转实际上可以看成一种对成变换

```cpp
class Solution {
public:
    string reverseStr(string s, int k) 
    {
        for (int i = 0; i < s.size(); i = i + 2 * k)
        {
            if (i + k <= s.size())
                reverse(s.begin() + i, s.begin() + i + k);
            else
                reverse(s.begin() + i, s.end());
        }
        return s;
    }
};
```



##### 2.4.3 左旋反转
![avastar](./picture/J58.png)
- 时间复杂度为 **O(n)**，空间复杂度为 **O(1)**
> 在普通反转的基础上，先全部反转，再部分反转

```cpp
class Solution {
public:
    string reverseLeftWords(string& s, int n) 
    {
        reverseall(s, 0, s.size()-1);
        reverseall(s, 0, s.size() - n - 1);
        reverseall(s, s.size() - n, s.size() - 1);
        return s;
    }

    void reverseall(string& s, int left, int right)
    {
        int start_index = left;
        int end_index = right;
        while (start_index <= end_index)
        {
            char temp = s[start_index];
            s[start_index] = s[end_index];
            s[end_index] = temp;
            start_index++;
            end_index--;
        }
    }
};
```

##### 2.4.4 花式反转
![avastar](./picture/151.png)
- 时间复杂度为 **O(n)**，空间复杂度为 **O(1)**
> 1. 先采用双指针法去除空格
> 2. 去除开头空格的思路与前面移除元素类似
> 3. 去除中间空格是当有连续两个空格时才去除，所以需要加入约束
> 4. 最后全部反转，再以空格为依据进行部分反转

```cpp
class Solution {
public:
    string reverseWords(string s) 
    {
        //去空格，采用双指针法移除元素的方法
        int slow_index = 0;
        int fast_index = 0;
        //去除开头空格
        while (s[fast_index] == ' ' && fast_index < s.size())
        {
            fast_index++;
        }
        //去除中间空格
        while (fast_index < s.size())
        {
            if (s[fast_index] == ' ' && s[fast_index] == s[fast_index + 1])
            {
                fast_index ++;
                continue;
            }
            s[slow_index] = s[fast_index];
            slow_index++;
            fast_index++;
        }
        //去除末尾空格
        if (s[slow_index - 1] == ' ')
            slow_index--;
        s.resize(slow_index);
        //全部反转
        reverseall(s, 0, slow_index - 1);
        //单词反转
        int next_start = 0;
        for (int i = 0; i < slow_index; i++)
        {
            if (s[i] == ' ')
            {   
                reverseall(s, next_start, i - 1);
                next_start = i + 1;
            }
        }
        //反转剩下的最后一个单词
        reverseall(s, next_start, slow_index - 1);
        return s;


    }

     void reverseall(string& s, int left, int right)
    {
        int start_index = left;
        int end_index = right;
        while (start_index <= end_index)
        {
            char temp = s[start_index];
            s[start_index] = s[end_index];
            s[end_index] = temp;
            start_index++;
            end_index--;
        }
    }
};
```

#### 2.5 替换字符
![avastar](./picture/J05.png)
- 时间复杂度为 **O(n)**，空间复杂度为 **O(1)**
> 1. 计算空格数预先给数组扩容带
> 2. 双指针法从后向前进行操作，避免了从前向后的子序列移动

```cpp
class Solution {
public:
    string replaceSpace(string s) 
    {
        int space_count = 0;
        //给空格计数
        for (int i = 0; i < s.size(); i++)
        {
            if (s[i] == ' ')
            {
                space_count++;
            }
        }
        //扩充字符串大小
        int old_size = s.size();
        s.resize(s.size() + space_count * 2);
        int new_size = s.size();
        //双指针法进行字符替换
        int slow_index = old_size - 1;
        int fast_index = new_size - 1;
        while (slow_index >= 0)
        {
            if (s[slow_index] == ' ')
            {
                s[fast_index--] = '0';
                s[fast_index--] = '2';
                s[fast_index--] = '%';
                slow_index--;                
            }
            else
            {
                s[fast_index] = s[slow_index];
                fast_index--;
                slow_index--; 
            }
        }
        return s;
    }
};
```

#### 2.6 两序列合并
![avastar](./picture/88.png)
- 双指针法时间复杂度为 **O(m+n)**，空间复杂度为 **O(1)**
> 1. 两有序序列各分配一个指针指向数组尾部
> 2. 双指针法**从后向前**进行操作，避免了从前向后的子序列移动，可以在较小的时间复杂度下，通过常数值的空间复杂度解决问题

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int s1_size, vector<int>& nums2, int s2_size)
    {
        int nums1_index = s1_size - 1;
        int nums2_index = s2_size - 1;
        int res_index = s1_size + s2_size - 1;
        while (nums1_index >= 0 || nums2_index >= 0)
        {
            if (nums1_index == -1)
            {
                nums1[res_index] = nums2[nums2_index];
                nums2_index--;
            }
            else if (nums2_index == -1)
            {
                nums1[res_index] = nums1[nums1_index];
                nums1_index--;
            }
            else if (nums1[nums1_index]  > nums2[nums2_index])
            {
                nums1[res_index] = nums1[nums1_index];
                nums1_index--;
            }
            else
            {
                nums1[res_index] = nums2[nums2_index];
                nums2_index--;
            }
            res_index--;
        }
       
    }
};
```

### 3. KMP算法
#### 3.1 常规搜索
![avastar](./picture/28.png)
- 时间复杂度为 **O(m+n)**，空间复杂度为 **O(n)**
> 1. 清楚next数组的含义
> 2. 在构造next数组时关注while语句中的回溯

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) 
    {   
        if (needle.size() == 0)
            return 0;
        //构建next数组
        //首先初始化
        int next[needle.size()];
        next[0] = 0;
        //双指针寻找子串的最长相等前后缀
        int prefix_index = 0;
        //第一个子序列的长度为2，末尾元素下标为1
        int suffix_index = 1;
        while (suffix_index < needle.size())
        {   

            //当在前缀表首元素之后出现不匹配的情况
            //回溯，回到前缀继续比较
            while (prefix_index > 0 && needle[prefix_index] != needle[suffix_index])
            {
                prefix_index = next[prefix_index - 1];
            }
            //找到的最长相等前缀的
            if (needle[prefix_index] == needle[suffix_index])
            {
                prefix_index++;
            }
            next[suffix_index] = prefix_index;
            suffix_index++;
        }

        //通过next数组进行匹配
        //通过next数组进行匹配
        int second_index = 0;
        for (int first_index = 0; first_index < haystack.size(); first_index++)
        {
            while (second_index > 0 && haystack[first_index] != needle[second_index])
            {
                second_index = next[second_index - 1];
            }
            if (haystack[first_index] == needle[second_index])
                second_index++;
            if (second_index == needle.size())
                return (first_index - needle.size() + 1);
        }
        return -1;
    }
};
```

#### 3.2 寻找子串
![avastar](./picture/459.png)
- 时间复杂度为 **O(n)**，空间复杂度为 **O(n)**
> 1. 清楚next数组的含义
> 2. 在构造next数组时关注while语句中的回溯

```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) 
    {
        //构建next数组
        //首先初始化
        int next[s.size()];
        next[0] = 0;
        //双指针寻找子串的最长相等前后缀
        int prefix_index = 0;
        //第一个子序列的长度为2，末尾元素下标为1
        int suffix_index = 1;
        while (suffix_index < s.size())
        {   

            //当在前缀表首元素之后出现不匹配的情况
            //回溯，回到前缀继续比较
            while (prefix_index > 0 && s[prefix_index] != s[suffix_index])
            {
                prefix_index = next[prefix_index - 1];
            }
            //找到的最长相等前缀的
            if (s[prefix_index] == s[suffix_index])
            {
                prefix_index++;
            }
            next[suffix_index] = prefix_index;
            suffix_index++;
        }
        if (next[s.size() - 1] != 0 && s.size() % (s.size() - next[s.size() - 1]) == 0)
        {
            return true;
        }
        return false;
    }
};
```

## 二、二维数组
### 2. 螺旋矩阵1
![avastar](./picture/54.png)
- 时间复杂度为 **O(n^2)**，空间复杂度为 **O(1)**
> 1. 抓住循环不变量如图中9个元素的示例，走“1，2”，“3，4”
> 2. 注意每次对循环不变量进行相同的变化

```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) 
    {
        vector<int> result;
        int rows = matrix.size();
        int cols = matrix[0].size();
        int loop = (rows > cols ? cols : rows) / 2;
        int flag = (rows > cols ? cols : rows) % 2;
        int offset = 1;
        int start_x = 0;
        int start_y = 0;
        int i,j;
        while (loop--)
        {
            i = start_x;
            j = start_y;
            for (j = start_y; j < start_y + cols - offset; j++)
            {
                result.push_back(matrix[i][j]);
            }
            
            for (i = start_x; i < start_x + rows - offset; i++)
            {
                result.push_back(matrix[i][j]);
            }

            for ( ; j > start_y; j--)
            {
                result.push_back(matrix[i][j]);
            }

            for ( ; i > start_x; i--)
            {
                result.push_back(matrix[i][j]);
            }
            offset = offset + 2;
            start_x++;
            start_y++;
        }

        if (flag == 1)
        {
            if (rows > cols)
            {
                for (int k = start_x; k < start_x + rows - offset + 1; k++)
                {
                    result.push_back(matrix[k][start_y]);
                }
            }
            else
            {
                for (int k = start_y; k < start_y + cols - offset + 1; k++)
                {
                    result.push_back(matrix[start_x][k]);
                }
            }
        }
        return result;
    }
};
```

### 2. 螺旋矩阵2
![avastar](./picture/59.png)
- 时间复杂度为 **O(n^2)**，空间复杂度为 **O(1)**
> 1. 抓住循环不变量如图中9个元素的示例，走“1，2”，“3，4”
> 2. 注意每次对循环不变量进行相同的变化

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) 
    {
        vector<vector<int>> matrix(n, vector<int>(n, 0));
        int loop = n/2;
        int count = 1;
        int offset = 1;
        int startx = 0;
        int starty = 0;
        int i,j;
        while (loop--)
        {
            i = startx;
            j = starty;
            for (j = starty; j < starty + n - offset; j++)
            {
                matrix[i][j] = count;
                count++;
            }
            
            for (i = startx; i < startx + n - offset; i++)
            {
                matrix[i][j] = count;
                count++;
            }

            for ( ; j > starty; j--)
            {
                matrix[i][j] = count;
                count++;
            }

            for ( ; i > startx; i--)
            {
                matrix[i][j] = count;
                count++;
            }

            offset = offset + 2;
            startx++;
            starty++;
        }
        if (n%2 == 1)
            matrix[n/2][n/2] = count;
        return matrix;
    }
};
```

