# LeetCode-Solution
记录自己提交且AC的代码<br>

## [654. 最大二叉树](https://leetcode-cn.com/problems/maximum-binary-tree/description/)
给定一个不含重复元素的整数数组。一个以此数组构建的最大二叉树定义如下：
1.   二叉树的根是数组中的最大元素。
2.   左子树是通过数组中最大值左边部分构造出的最大二叉树。
3.   右子树是通过数组中最大值右边部分构造出的最大二叉树。
通过给定的数组构建最大二叉树，并且输出这个树的根节点。<br>

**Example 1:**
```bash
输入: [3,2,1,6,0,5]
输入: 返回下面这棵树的根节点：

      6
    /   \
   3     5
    \    / 
     2  0   
       \
        1
```
注意:<br>
1.给定的数组的大小在 [1, 1000] 之间。

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        if (nums.empty())
        {
            return nullptr;
        }
        
        TreeNode* pRoot = new TreeNode(nums[0]);
        for (int nIndex = 1; nIndex < nums.size(); ++nIndex)
        {
            TreeNode* pNode = new TreeNode(nums[nIndex]);
            if (nums[nIndex] > pRoot->val)
            {
                pNode->left = pRoot;
                pRoot = pNode;
                continue;
            }
            
            AddNode(pRoot, pRoot->right, pNode);
        }
        
        return pRoot;
    }
    
    void AddNode(TreeNode* pParent, TreeNode* pChild, TreeNode* pNode)
    {
        if (pChild == nullptr)
        {
            pParent->right = pNode;
            return;
        }
        
        if (pChild->val < pNode->val)
        {
            pNode->left = pChild;
            pParent->right = pNode;
            return;
        }
        
        AddNode(pChild, pChild->right, pNode);
    }
};
```



## [771. 宝石与石头](https://leetcode-cn.com/problems/jewels-and-stones/description/)
给定字符串`J`代表石头中宝石的类型，和字符串`S`代表你拥有的石头。`S`中每个字符代表了一种你拥有的石头的类型，你想知道你拥有的石头中有多少是宝石。
`J`中的字母不重复，`J`和`S`中的所有字符都是字母。字母区分大小写，因此`"a"`和`"A"`是不同类型的石头。<br>
**示例 1:**
```bash
输入: J = "aA", S = "aAAbbbb"
输出: 3
```
**示例 2:**
```bash
输入: J = "z", S = "ZZ"
输出: 0
```
注意:
*   `S`和`J`最多含有50个字母。
*   `J`中的字符不重复。

```cpp
class Solution {
public:
    int numJewelsInStones(string J, string S) {
        map<int, int> mapCharCount;
        for (int nIndex = 0; nIndex < S.size(); ++nIndex)
        {
            ++mapCharCount[S[nIndex]];
        }
        
        int nCount = 0;
        for (int nIndex = 0; nIndex < J.size(); ++nIndex)
        {
            nCount += mapCharCount[J[nIndex]];
        }
        
        return nCount;
    }
};
```
