<!--
 * @Author: yinzhicun
 * @Date: 2021-04-18 10:08:24
 * @LastEditTime: 2021-04-18 13:26:28
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /Leetcode_Note/data_structure/note_btree.md
-->
# <center>二叉树习题</center>

## 一、二叉树的遍历
四种遍历方式
### 1. 前序遍历
![](./picture/144.png)
> 前序遍历就是中左右的顺序
递归法
- 时间复杂度为 **O(n)** ，空间复杂度为 **O(n)**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> res;

    vector<int> preorderTraversal(TreeNode* root) {
        if (!root) return res;
        res.push_back(root->val);
        preorderTraversal(root->left);
        preorderTraversal(root->right);
        return res;
    }
};
```

迭代法
- 时间复杂度为 **O(n)** ，空间复杂度为 **O(n)**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) 
    {
        vector<int> res;
        if(!root) 
            return res;
        stack<TreeNode*> stk;
        stk.push(root);
        while(!stk.empty())
        {
            TreeNode* tmp = stk.top();
            stk.pop();
            res.push_back(tmp -> val);
            if (tmp->right)
                stk.push(tmp->right);
            if (tmp->left)
                stk.push(tmp->left);
        }
        return res; 
    }
};
```

### 2. 中序遍历
![](./picture/94.png)
> 中序遍历就是左中右的顺序
递归法
- 时间复杂度为 **O(n)** ，空间复杂度为 **O(n)**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> result;
    vector<int> inorderTraversal(TreeNode* root) 
    {
        if (root == nullptr)
            return result;
        inorderTraversal(root->left);
        result.push_back(root->val);
        inorderTraversal(root->right);
        return result;
    }
};
```

迭代法
- 时间复杂度为 **O(n)** ，空间复杂度为 **O(n)**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) 
    {
        vector<int> res;
        stack<TreeNode*> stk;
        while (root != nullptr || !stk.empty()) 
        {
            while (root != nullptr) 
            {
                stk.push(root);
                root = root->left;
            }
            root = stk.top();
            stk.pop();
            res.push_back(root->val);
            root = root->right;
        }
        return res;
    }
};
```

### 3. 后序遍历
![](./picture/145.png)
> 后序遍历就是左右中的顺序
递归法
- 时间复杂度为 **O(n)** ，空间复杂度为 **O(n)**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> res;
    vector<int> postorderTraversal(TreeNode* root) 
    {
        if (root == nullptr)
            return res;
        postorderTraversal(root->left);
        postorderTraversal(root->right);
        res.push_back(root->val);
        return res;
    }
};
```

迭代法
- 时间复杂度为 **O(n)** ，空间复杂度为 **O(n)**
> 实际上前序是中左右，修改一下写法变为中右左的顺序，在反转一下就可以得到后序遍历了

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) 
    {
        vector<int> res;
        if(!root) 
            return res;
        stack<TreeNode*> stk;
        stk.push(root);
        while(!stk.empty())
        {
            TreeNode* tmp = stk.top();
            stk.pop();
            res.push_back(tmp -> val);
            if (tmp->left)
                stk.push(tmp->left);
            if (tmp->right)
                stk.push(tmp->right);
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```

### 4. 三种遍历迭代法的统一写法
实际上三种遍历的方式就是根据树的递归性质得到的，通过迭代法进行三种遍历，实际上就是通过栈的数据结构对递归算法中的栈进行模拟。
> 统一写法的关键在于使用一个NULL节点将遍历节点与处理元素的矛盾化解
 
> 理解3点：以中序遍历为例：
> 1. 栈的特性入栈和出栈顺序相反，想要输出顺序左中右，入栈顺序必须按照右中左。
> 2. 入栈的处理：可以把整颗树简化为3个节点一组的多个子树。即(父节点，左孩子，右孩子)这3个节点组成的子树。每次循环处理的实际就是将这样的3个节点按照规则顺序(右中左)进行入栈。所以才有了代码中看到的：每次都是先将栈顶元素去除，然后对以栈顶元素做为父节点的3个节点子树按规则顺序（右中左）入栈。
> 3. NULL节点的加入和出栈规则的规定：保证了当左孩子作为栈顶元素时，不会立即出栈，而是会将当前的左孩子(即栈顶元素)作为下次遍历的父节点接着按照规则顺序入栈。直到当前的左孩子做为父节点再无孩子时(无孩子时，入栈规则就成了(父节点,NULL节点))，遇到NULL节点了，才进行出栈。这样就保证了左孩子先出栈

- 中序遍历

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) 
    {
        vector<int> res;
        stack<TreeNode*> stk;
        if (root)
            stk.push(root);
        while (!stk.empty()) 
        {
            TreeNode* node = stk.top();
            if (node != nullptr) 
            {
                stk.pop();
                //顺序要求为左中右，所以按右中压栈
                if (node->right)
                    stk.push(node->right);
                stk.push(node);
                stk.push(nullptr);
                if (node->left)
                    stk.push(node->left);
            }
            else
            {
                stk.pop();
                res.push_back(stk.top()->val);
                stk.pop();
            }
        }
        return res;
    }
};
```

- 前序遍历

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) 
    {
        vector<int> res;
        stack<TreeNode*> stk;
        if (root)
            stk.push(root);
        while (!stk.empty()) 
        {
            TreeNode* node = stk.top();
            if (node != nullptr) 
            {
                stk.pop();
                //顺序要求为中左右，所以按右左中压栈
                if (node->right)
                    stk.push(node->right);
                if (node->left)
                    stk.push(node->left);
                stk.push(node);
                stk.push(nullptr);
            }
            else
            {
                stk.pop();
                res.push_back(stk.top()->val);
                stk.pop();
            }
        }
        return res;
    }
};
```

- 后序遍历

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) 
    {
        vector<int> res;
        stack<TreeNode*> stk;
        if (root)
            stk.push(root);
        while (!stk.empty()) 
        {
            TreeNode* node = stk.top();
            if (node != nullptr) 
            {
                stk.pop();
                //顺序要求为左右中，所以按中右左压栈
                stk.push(node);
                stk.push(nullptr);
                if (node->right)
                    stk.push(node->right);
                if (node->left)
                    stk.push(node->left);

            }
            else
            {
                stk.pop();
                res.push_back(stk.top()->val);
                stk.pop();
            }
        }
        return res;
    }
};
```

### 5. 层序遍历

## 二、栈
