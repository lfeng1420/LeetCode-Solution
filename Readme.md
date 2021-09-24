# LeetCode-Solution
记录自己提交且AC的代码<br>
<!-- vscode-markdown-toc -->
* [1. 两数之和](#1.-两数之和)
* [2. 两数相加](#2.-两数相加)
* [7. 反转整数](#7.-反转整数)
* [8. 字符串转整数 (atoi)](#8.-字符串转整数-(atoi))
* [23. 合并K个升序链表](#23.-合并k个升序链表)
* [25. K 个一组翻转链表](#25.-k-个一组翻转链表)
* [33. 搜索旋转排序数组](#33.-搜索旋转排序数组)
* [52. N皇后 II](#52.-n皇后-ii)
* [101. 对称二叉树](#101.-对称二叉树)
* [146. LRU 缓存机制](#146.-lru-缓存机制)
* [206. 反转链表](#206.-反转链表)
* [215. 数组中的第K个最大元素](#215.-数组中的第k个最大元素)
* [654. 最大二叉树](#654.-最大二叉树)
* [657. 判断路线成圈](#657.-判断路线成圈)
* [771. 宝石与石头](#771.-宝石与石头)
* [807. 保持城市天际线](#807.-保持城市天际线)
* [814. 二叉树剪枝](#814.-二叉树剪枝)
* [912. 排序数组](#912.-排序数组)

<!-- vscode-markdown-toc-config
	numbering=false
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc --><br>

## <a name='1.-两数之和'></a>1. 两数之和
[题目链接](https://leetcode-cn.com/problems/two-sum)

**Code:**
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int, vector<int>> mapNums;
        for (int nIndex = 0; nIndex < nums.size(); ++nIndex)
        {
            mapNums[nums[nIndex]].push_back(nIndex);
        }

        vector<int> vecResult;
        map<int, vector<int>>::iterator itNums = mapNums.begin();
        for (; itNums != mapNums.end(); ++itNums)
        {
            int nOtherOne = target - itNums->first;
            map<int, vector<int>>::iterator itNumsOther = mapNums.find(nOtherOne);
            if (itNumsOther != mapNums.end())
            {
                if (itNumsOther != itNums)
                {
                    vecResult.push_back(itNums->second[0]);
                    vecResult.push_back(itNumsOther->second[0]);
                }
                else
                {
                    vecResult.push_back(itNums->second[0]);
                    vecResult.push_back(itNums->second[1]);
                }
                return vecResult;
            }
        }

        return vecResult;
    }
};
```

## <a name='2.-两数相加'></a>2. 两数相加
[题目链接](https://leetcode-cn.com/problems/add-two-numbers)

**Code:**
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* pHead = nullptr;
        ListNode* pCur = nullptr;
        int nTemp = 0;
        for (; l1 != nullptr && l2 != nullptr; l1 = l1->next, l2 = l2->next)
        {
            int nVal = (l1->val + l2->val + nTemp) % 10;
            nTemp = (l1->val + l2->val + nTemp) / 10;
            if (pHead == nullptr)
            {
                pHead = new ListNode(nVal);
                pCur = pHead;
                continue;
            }

            pCur->next = new ListNode(nVal);
            pCur = pCur->next;
        }

        ListNode* pRemain = (l1 != nullptr) ? l1 : l2;
        for (; pRemain != nullptr; pRemain = pRemain->next)
        {
            int nVal = (pRemain->val + nTemp) % 10;
            nTemp = (pRemain->val + nTemp) / 10;
            pCur->next = new ListNode(nVal);
            pCur = pCur->next;
        }
        
        if (nTemp > 0)
        {
            pCur->next = new ListNode(nTemp);
        }

        return pHead;
    }
};
```

## <a name='7.-反转整数'></a>7. 反转整数
[题目链接](https://leetcode-cn.com/problems/reverse-integer)

**Code:**
```cpp
class Solution {
public:
    int reverse(int x) {
        int nVal = 0;
        bool bPositiveFlag = (x >= 0);
        while (x != 0)
        {
            int nNewVal = nVal * 10 + (x % 10);
            if ((bPositiveFlag && nNewVal < nVal)
               || (!bPositiveFlag && nNewVal > nVal)
               || ((nNewVal - x % 10) / 10 != nVal))
            {
                return 0;
            }
            
            x /= 10;
            nVal = nNewVal;
        }
        
        return nVal;
    }
};
```

## <a name='8.-字符串转整数-(atoi)'></a>8. 字符串转整数 (atoi)
[题目链接](https://leetcode-cn.com/problems/string-to-integer-atoi)

**Code:**
```cpp
class Solution {
public:
    int myAtoi(string str) {
        int nVal = 0;
        bool bStartFlag = false;
        bool bPositiveFlag = true;
        for (int nIndex = 0; nIndex < str.size(); ++nIndex)
        {
            char ch = str[nIndex];
            bool bNumFlag = (ch >= '0' && ch <= '9');
            if (!bStartFlag)
            {
                if (ch == ' ')
                {
                    continue;
                }
                
                if (ch == '-' || ch == '+')
                {
                    bPositiveFlag = (ch == '+');
                    bStartFlag = true;
                    continue;
                }
                else if (!bNumFlag)
                {
                    break;
                }
                
                bStartFlag = true;
            }
            
            if (!bNumFlag)
            {
                break;
            }
            
            int nOne = (bPositiveFlag ? 1 : -1) * (ch - '0');
            int nNewVal = nVal * 10 + nOne;
            if ((nNewVal - nOne) / 10 != nVal)
            {
                return (bPositiveFlag ? INT_MAX : INT_MIN);
            }
            else if (bPositiveFlag && nNewVal < nVal)
            {
                return INT_MAX;
            }
            else if (!bPositiveFlag && nNewVal > nVal)
            {
                return INT_MIN;
            }
            nVal = nNewVal;
        }
        
        return nVal;
    }
};
```

## <a name='23.-合并k个升序链表'></a>23. 合并K个升序链表
[题目链接](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

**Code:**
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode* head = nullptr;
        ListNode* cur = nullptr;
        int index;
        while ((index = getMinNode(lists)) != -1)
        {
            ListNode*& p = lists[index];
            if (cur != nullptr) cur->next = p;
            cur = p;
            p = p->next;
            if (head == nullptr) head = cur;
        }

        return head;
    }

    int getMinNode(vector<ListNode*>& lists)
    {
        int nMin = INT_MAX;
        int index = -1;
        for (int idx = 0; idx < lists.size(); ++idx)
        {
            if (lists[idx] != nullptr 
                && lists[idx]->val < nMin)
            {
                index = idx;
                nMin = lists[idx]->val;
            }
        }

        return index;
    }
};
```

## <a name='25.-k-个一组翻转链表'></a>25. K 个一组翻转链表
[题目链接](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

**Code:**
```cpp
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode* rhead = nullptr;
        ListNode* first = nullptr;
        ListNode* llast = nullptr;
        for (int index = 1; head != nullptr; ++index)
        {
            if (first == nullptr)
            {
                first = head;
            }
            if (index != k)
            {
                head = head->next;
                continue;
            }

            ListNode* pre = nullptr;
            ListNode* cur = first;
            while (cur != head)
            {
                ListNode* next = cur->next;
                cur->next = pre;
                pre = cur;
                cur = next;
            }

            ListNode* next = head->next;
            head->next = pre;
            if (rhead == nullptr)
            {
                rhead = head;
            }

            if (llast != nullptr)
                llast->next = head;
            llast = first;
            index = 0;
            first = nullptr;
            head = next;
        }

        if (llast != nullptr)
            llast->next = first;
        return rhead;
    }
};
```

## <a name='33.-搜索旋转排序数组'></a>33. 搜索旋转排序数组
[题目链接](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

**Code:**
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int last = nums.size() - 1;
        if (target == nums[0]) return 0;
        if (target == nums[last]) return last;
        int k = getK(nums, 0, last);
        if (k == -1) return -1;
        bool bGT = (target > nums[0]);
        return binarySearch(nums.data(), target, bGT ? 0 : k + 1, bGT ? k : nums.size() - 1);
    }

    int getK(vector<int>& nums, int s, int e)
    {
        if (s == e) return s;
        int m = (s + e) / 2;
        if (nums[m] >= nums[s]) return (nums[m] < nums[e]) ? getK(nums, min(m + 1, e), e) : getK(nums, m, max(e - 1, m));
        else if (nums[m] < nums[e]) return getK(nums, s, max(m - 1, 0));
        return -1;
    }

    int binarySearch(int* arr, int target, int s, int e)
    {
        if (s > e) return -1;
        int m = (s + e) /2;
        const int& val = arr[m];
        if (target == val) return m;
        return (target > val) ? binarySearch(arr, target, m + 1, e) : 
            binarySearch(arr, target, s, m - 1);
    }
};
```


## <a name='52.-n皇后-ii'></a>52. N皇后 II
[题目链接](https://leetcode-cn.com/problems/n-queens-ii)

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

## <a name='101.-对称二叉树'></a>101. 对称二叉树
[题目链接](https://leetcode-cn.com/problems/symmetric-tree/)

**Code:**
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
    bool isSymmetric(TreeNode* root) {
        //return isEqual(root->left, root->right);
        return checkEqual(root->left, root->right);
    }

    bool checkEqual(TreeNode* left, TreeNode* right)
    {
        list<TreeNode*> leftNodes {left};
        list<TreeNode*> rightNodes {right};
        while (!leftNodes.empty() && !rightNodes.empty())
        {
            left = leftNodes.back();
            leftNodes.pop_back();
            right = rightNodes.back();
            rightNodes.pop_back();
            if (left == nullptr && right == nullptr) continue;
            if ((left != nullptr && right == nullptr)
                || (left == nullptr && right != nullptr)
                || left->val != right->val) return false;
            leftNodes.emplace_back(left->left);
            leftNodes.emplace_back(left->right);
            rightNodes.emplace_back(right->right);
            rightNodes.emplace_back(right->left);
        }

        return leftNodes.empty() && rightNodes.empty();
    }

    bool isEqual(TreeNode* left, TreeNode* right)
    {
        if (left == nullptr) return right == nullptr;
        if (right == nullptr) return left == nullptr;
        if (left->val != right->val) return false;
        return isEqual(left->left, right->right) && isEqual(left->right, right->left);
    }
};
```

## <a name='146.-lru-缓存机制'></a>146. LRU 缓存机制
[题目链接](https://leetcode-cn.com/problems/lru-cache/)

**Code:**
```cpp
class LRUCache {
public:
    LRUCache(int capacity) : m_nCap(capacity), m_nCount(0), m_pHead(nullptr), m_pTail(nullptr) {
    }

    int get(int key) {
        auto it = m_mapVal.find(key);
        if (it == m_mapVal.end())
        {
            return -1;
        }

        _SNode& stNode = it->second;
        if (stNode.nVal != -1)
        {
            removeNode(stNode);
            appendToTail(stNode);
        }

        return stNode.nVal;
    }

    void put(int key, int value) {
        _SNode& stNode = m_mapVal[key];
        if (stNode.nVal != -1)
        {
            stNode.nVal = value;
            removeNode(stNode);
            appendToTail(stNode);
            return;
        }

        if (m_nCount == m_nCap)
        {
            _SNode* pHead = m_pHead;
            pHead->nVal = -1;
            removeNode(*pHead);
            m_mapVal.erase(pHead->nKey);
        }
        else
        {
            ++m_nCount;
        }

        stNode.nKey = key;
        stNode.nVal = value;
        appendToTail(stNode);
    }

private:
    struct _SNode;
    void removeNode(_SNode& stNode)
    {
        if (stNode.pPre != nullptr)
        {
            stNode.pPre->pNext = stNode.pNext;
        }
        if (stNode.pNext != nullptr)
        {
            stNode.pNext->pPre = stNode.pPre;
        }

        if (m_pTail == &stNode)
        {
            m_pTail = stNode.pPre;
        }
        if (m_pHead == &stNode)
        {
            m_pHead = stNode.pNext;
        }

        stNode.pPre = nullptr;
        stNode.pNext = nullptr;
    }

    void appendToTail(_SNode& stNode)
    {
        if (m_pTail == nullptr)
        {
            m_pTail = &stNode;
            m_pHead = m_pTail;
            return;
        }

        if (m_pTail == &stNode)
        {
            return;
        }

        m_pTail->pNext = &stNode;
        stNode.pNext = nullptr;
        stNode.pPre = m_pTail;
        m_pTail = &stNode;
    }

private:
    struct _SNode
    {
        _SNode* pPre;
        int nKey;
        int nVal;
        _SNode* pNext;

        _SNode() : pPre(nullptr), nKey(-1), nVal(-1), pNext(nullptr) {}
    };

private:
    std::unordered_map<int, _SNode> m_mapVal;
    int m_nCap;
    int m_nCount;
    _SNode* m_pHead;
    _SNode* m_pTail;
};

/*
testcases:
["LRUCache","put","put","get","put","get","put","get","get","get"]
[[2],[1,1],[2,2],[1],[3,3],[2],[4,4],[1],[3],[4]]
["LRUCache","put","put","get","get","put","get","get","get"]
[[2],[2,1],[3,2],[3],[2],[4,3],[2],[3],[4]]
["LRUCache","put","get","put","get","get"]
[[1],[2,1],[2],[3,2],[2],[3]]
*/
/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

## <a name='206.-反转链表'></a>206. 反转链表
[题目链接](https://leetcode-cn.com/problems/reverse-linked-list/)

**Code:**
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* pCur = head;
        ListNode* pNext = nullptr;
        while (pCur != nullptr)
        {
            ListNode* pTmp = pCur->next;
            pCur->next = pNext;
            pNext = pCur;
            pCur = pTmp;
        }

        return pNext;
    }
};
```

## <a name='215.-数组中的第k个最大元素'></a>215. 数组中的第K个最大元素
[题目链接](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

给定整数数组 nums 和整数 k，请返回数组中第 k 个最大的元素。
请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。
**示例：**
```
示例 1:
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5

示例 2:
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```

**提示：**
```
1 <= k <= nums.length <= 10^4
-10^4 <= nums[i] <= 10^4
```

**Code:**
```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        std::sort(nums.begin(), nums.end(), std::greater<int>());
        return nums[k-1];
    }
};
```


## <a name='654.-最大二叉树'></a>654. 最大二叉树
[题目链接](https://leetcode-cn.com/problems/maximum-binary-tree)

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

## <a name='657.-判断路线成圈'></a>657. 判断路线成圈
[题目链接](https://leetcode-cn.com/problems/judge-route-circle)

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

## <a name='771.-宝石与石头'></a>771. 宝石与石头
[题目链接](https://leetcode-cn.com/problems/jewels-and-stones)

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

## <a name='807.-保持城市天际线'></a>807. 保持城市天际线
[题目链接](https://leetcode-cn.com/problems/max-increase-to-keep-city-skyline)

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

## <a name='814.-二叉树剪枝'></a>814. 二叉树剪枝
[题目链接](https://leetcode-cn.com/problems/binary-tree-pruning)

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
    TreeNode* pruneTree(TreeNode* root) {
        if (root != nullptr)
        {
            updateTree(root->left, root);
            updateTree(root->right, root);
        }
        
        return root;
    }
    
    void updateTree(TreeNode* pCur, TreeNode* pParent)
    {
        if (pCur == nullptr)
        {
            return;
        }
        
        updateTree(pCur->left, pCur);
        updateTree(pCur->right, pCur);
        
        if (pCur->val == 0 && pCur->left == nullptr && pCur->right == nullptr)
        {
            if (pParent->left == pCur)
            {
                pParent->left = nullptr;
            }
            else if (pParent->right == pCur)
            {
                pParent->right = nullptr;
            }
        }
    }
};
```

## <a name='912.-排序数组'></a>912. 排序数组
[题目链接](https://leetcode-cn.com/problems/sort-an-array/)

**Code:**
```cpp
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        srand((unsigned int)time(NULL));
        //quick_sort(nums.data(), 0, nums.size()-1);
        //shell_sort(nums.data(), nums.size());
        static vector<int> s_vecNums;
        s_vecNums.resize(nums.size(), 0);
        mergeSort(nums.data(), s_vecNums.data(), 0, nums.size() - 1);
        return nums;
    }

    void quick_sort(int* arr, int start, int end)
    {
        if (start >= end) return;
        int l = start;
        int r = end;
        int t = rand() % (end - start + 1) + start;
        swap(arr[t], arr[l]);
        int base = arr[l];
        while (l < r)
        {
            while (l < r)
            {
                if (arr[r] < base)
                {
                    arr[l++] = arr[r];
                    break;
                }

                --r;
            }

            while (l < r)
            {
                if (arr[l] >= base)
                {
                    arr[r--] = arr[l];
                    break;
                }

                ++l;
            }
        }

        arr[l] = base;
        quick_sort(arr, start, l-1);
        quick_sort(arr, r+1, end);
    }

    void shell_sort(int* arr, int num)
    {
        for (int step = num / 2; step > 0; step /= 2)
        {
            for (int nIndex = step; nIndex < num; ++nIndex)
            {
                for (int nIdx = nIndex - step; nIdx >= 0; nIdx -= step)
                {
                    if (arr[nIdx] > arr[nIdx + step])
                    {
                        swap(arr[nIdx], arr[nIdx + step]);
                    }
                }
            }
        }
    }

    void mergeSort(int* arr, int* temp, int s, int e)
    {
        if (s == e) return;
        int mid = (s + e) /2;
        mergeSort(arr, temp, s, mid);
        mergeSort(arr, temp, mid + 1, e);

        int s1 = s, s2=mid+1, len = 0;
        while (s1 <= mid && s2 <= e)
        {
            if (arr[s1] < arr[s2])
            {
                temp[len++] = arr[s1++];
                continue;
            }

            temp[len++] = arr[s2++];
        }

        while (s1 <= mid)
            temp[len++] = arr[s1++];
        while (s2 <= e)
            temp[len++] = arr[s2++];
        for (int idx = 0; idx < len; ++idx)
        {
            arr[idx + s] = temp[idx];
        }
    }
};
```