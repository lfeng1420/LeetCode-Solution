# LeetCode-Solution
记录自己提交且AC的代码<br>

## [52. N皇后 II](https://leetcode-cn.com/problems/n-queens-ii/description/)
n 皇后问题研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。
![上图为 8 皇后问题的一种解法。](images/8-queens.png)<br>
给定一个整数 n，返回 n 皇后不同的解决方案的数量。<br>
**示例:**
```bash
输入: 4
输出: 2
解释: 4 皇后问题存在如下两个不同的解法。
[
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
```

**Code:**
```cpp
class Solution {
public:
    int totalNQueens(int n) {
        int nCount = 0;
        int nCurIdx = 0;
        vector<int> vecQueens;
        recursiveWays(vecQueens, nCount, nCurIdx, n);
        return nCount;
    }
    
    void recursiveWays(vector<int>& vecQueens, int& nCount, int nCurIdx, int n)
    {
        for (int nIndex = 0; nIndex < n; ++nIndex)
        {
            if (canAttack(vecQueens, nCurIdx, nIndex))
            {
                continue;
            }

            if (nCurIdx == n - 1)
            {
                ++nCount;
                continue;
            }

            vecQueens.push_back(nIndex);
            recursiveWays(vecQueens, nCount, nCurIdx + 1, n);
            vecQueens.erase(vecQueens.begin() + nCurIdx);
        }
    }
    
    bool canAttack(const vector<int>& vecQueens, int nCurIdx, int nCurPos)
    {
        if (!vecQueens.empty())
        {
            for (int nIndex = 0; nIndex < vecQueens.size(); ++nIndex)
            {
                int nLastPos = vecQueens[nIndex];
                if (nLastPos == nCurPos 
                    || abs(nCurIdx - nIndex) == abs(nCurPos - nLastPos))
                {
                    return true;
                }
            }
        }

        return false;
    }
};
```

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

**Code:**
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

## [657. 判断路线成圈](https://leetcode-cn.com/problems/judge-route-circle/description/)
初始位置 (0, 0) 处有一个机器人。给出它的一系列动作，判断这个机器人的移动路线是否形成一个圆圈，换言之就是判断它是否会移回到原来的位置。
移动顺序由一个字符串表示。每一个动作都是由一个字符来表示的。机器人有效的动作有 R（右），L（左），U（上）和 D（下）。输出应为 true 或 false，表示机器人移动路线是否成圈。<br>
**示例 1:**
```bash
输入: "UD"
输出: true
```
**示例 2:**
```bash
输入: "LL"
输出: false
```

**Code:**
```cpp
class Solution {
public:
    bool judgeCircle(string moves) {
        int nLeftCount = 0;
        int nUpCount = 0;
        for (int nIndex = 0; nIndex < moves.size(); ++nIndex)
        {
            if (moves[nIndex] == 'U')
            {
                ++nUpCount;
            }
            else if (moves[nIndex] == 'D')
            {
                --nUpCount;
            }
            else if (moves[nIndex] == 'L')
            {
                ++nLeftCount;
            }
            else if (moves[nIndex] == 'R')
            {
                --nLeftCount;
            }
        }
        
        return (nLeftCount == 0 && nUpCount == 0);
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

**Code:**
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

## [807. 保持城市天际线](https://leetcode-cn.com/problems/max-increase-to-keep-city-skyline/description/)
在二维数组`grid`中，`grid[i][j]`代表位于某处的建筑物的高度。 我们被允许增加任何数量（不同建筑物的数量可能不同）的建筑物的高度。 高度 0 也被认为是建筑物。
最后，从新数组的所有四个方向（即顶部，底部，左侧和右侧）观看的“天际线”必须与原始数组的天际线相同。 城市的天际线是从远处观看时，由所有建筑物形成的矩形的外部轮廓。 请看下面的例子。
建筑物高度可以增加的最大总和是多少？<br>
```bash
例子：
输入： grid = [[3,0,8,4],[2,4,5,7],[9,2,6,3],[0,3,1,0]]
输出： 35
解释： 
The grid is:
[ [3, 0, 8, 4], 
  [2, 4, 5, 7],
  [9, 2, 6, 3],
  [0, 3, 1, 0] ]

从数组竖直方向（即顶部，底部）看“天际线”是：[9, 4, 8, 7]
从水平水平方向（即左侧，右侧）看“天际线”是：[8, 7, 9, 3]

在不影响天际线的情况下对建筑物进行增高后，新数组如下：

gridNew = [ [8, 4, 8, 7],
            [7, 4, 7, 7],
            [9, 4, 8, 7],
            [3, 3, 3, 3] ]
```
<br>
**说明:**
*   `1 < grid.length = grid[0].length <= 50`。
*   `grid[i][j]` 的高度范围是：`[0, 100]`。
*   一座建筑物占据一个`grid[i][j]`：换言之，它们是`1 x 1 x grid[i][j]`的长方体。
<br>
**Code:**
```cpp
class Solution {
public:
    int maxIncreaseKeepingSkyline(vector<vector<int>>& grid) {
        int nGridSize = grid.size();
        vector<int> vecHorizontalLine(nGridSize);
        vector<int> vecVerticalLine(nGridSize);
        for (int i = 0; i < grid.size(); ++i)
        {
            for (int j = 0; j < grid[i].size(); ++j)
            {
                vecVerticalLine[j] = max(vecVerticalLine[j], grid[i][j]);
                vecHorizontalLine[i] = max(vecHorizontalLine[i], grid[i][j]);
            }
        }

        int nTotalVal = 0;
        for (int i = 0; i < grid.size(); ++i)
        {
            for (int j = 0; j < grid[i].size(); ++j)
            {
                int nNewVal = min(vecHorizontalLine[i], vecVerticalLine[j]);
                nTotalVal += (nNewVal - grid[i][j]);
                grid[i][j] = nNewVal;
            }
        }

        return nTotalVal;
    }
};
```