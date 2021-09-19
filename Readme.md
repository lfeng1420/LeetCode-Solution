# LeetCode-Solution
记录自己提交且AC的代码<br>
<br>

## [1. 两数之和](https://leetcode-cn.com/problems/two-sum/description/)
给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。<br>
你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。<br>
**示例:**
```bash
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```
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

## [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/description/)
给定两个非空链表来表示两个非负整数。位数按照逆序方式存储，它们的每个节点只存储单个数字。将两数相加返回一个新的链表。<br>
你可以假设除了数字 0 之外，这两个数字都不会以零开头。<br>
**示例:**
```bash
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```
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

## [7. 反转整数](https://leetcode-cn.com/problems/reverse-integer/description/)
给定一个 32 位有符号整数，将整数中的数字进行反转。<br>
**示例 1:**
```bash
输入: 123
输出: 321
```
**示例 2:**
```bash
输入: -123
输出: -321
```
**示例 3:**
```bash
输入: 120
输出: 21
```
**说明：**<br>
假设我们的环境只能存储`32`位有符号整数，其数值范围是`[-2^31,  2^31 - 1]`。根据这个假设，如果反转后的整数溢出，则返回 0。<br>

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

## [8. 字符串转整数 (atoi)](https://leetcode-cn.com/problems/string-to-integer-atoi/description/)
实现`atoi`，将字符串转为整数。<br>
在找到第一个非空字符之前，需要移除掉字符串中的空格字符。如果第一个非空字符是正号或负号，选取该符号，并将其与后面尽可能多的连续的数字组合起来，这部分字符即为整数的值。如果第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。<br>
字符串可以在形成整数的字符后面包括多余的字符，这些字符可以被忽略，它们对于函数没有影响。<br>
当字符串中的第一个非空字符序列不是个有效的整数；或字符串为空；或字符串仅包含空白字符时，则不进行转换。<br>
若函数不能执行有效的转换，返回`0`。<br>
**说明：**<br>
假设我们的环境只能存储`32`位有符号整数，其数值范围是`[-2^31,  2^31 - 1]`。如果数值超过可表示的范围，则返回`INT_MAX (2^31 - 1)`或`INT_MIN (-2^31)`。

**示例 1:**
```bash
输入: "42"
输出: 42
```
**示例 2:**
```bash
输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
```
**示例 3:**
```bash
输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
```
**示例 4:**
```bash
输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
```
**示例 5:**
```bash
输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。
```

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

## [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)
给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。
k 是一个正整数，它的值小于或等于链表的长度。
如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。
进阶：
你可以设计一个只使用常数额外空间的算法来解决此问题吗？
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。
**示例：**
```
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]
输入：head = [1,2,3,4,5], k = 1
输出：[1,2,3,4,5]
输入：head = [1], k = 1
输出：[1]
```
**提示：**
```
列表中节点的数量在范围 sz 内
1 <= sz <= 5000
0 <= Node.val <= 1000
1 <= k <= sz
```

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

## [52. N皇后 II](https://leetcode-cn.com/problems/n-queens-ii/description/)
n 皇后问题研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。<br>
![上图为 8 皇后问题的一种解法。](images/52_8_queens.png)<br>
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

## [146. LRU 缓存机制](https://leetcode-cn.com/problems/lru-cache/)
运用你所掌握的数据结构，设计和实现一个`LRU (最近最少使用) 缓存机制`。
实现 `LRUCache` 类：
* `LRUCache(int capacity)` 以正整数作为容量 `capacity` 初始化 `LRU` 缓存
* `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
* `void put(int key, int value)` 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字-值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

**示例：**
```
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```

**提示：**
```
1 <= capacity <= 3000
0 <= key <= 10000
0 <= value <= 10^5
最多调用 2 * 10^5 次 get 和 put
```

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

## [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)
给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。
```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
输入：head = [1,2]
输出：[2,1]
输入：head = []
输出：[]
```

**提示：**
```
提示：
链表中节点的数目范围是 [0, 5000]
-5000 <= Node.val <= 5000
```

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

## [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)
给定整数数组 nums 和整数 k，请返回数组中第 k 个最大的元素。
请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。
**示例：**
```
示例 1:
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5

示例 2:
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


## [654. 最大二叉树](https://leetcode-cn.com/problems/maximum-binary-tree/description/)
给定一个不含重复元素的整数数组。一个以此数组构建的最大二叉树定义如下：<br>
1.   二叉树的根是数组中的最大元素。<br>
2.   左子树是通过数组中最大值左边部分构造出的最大二叉树。<br>
3.   右子树是通过数组中最大值右边部分构造出的最大二叉树。<br>
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
1.给定的数组的大小在 [1, 1000] 之间。<br>

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
初始位置 (0, 0) 处有一个机器人。给出它的一系列动作，判断这个机器人的移动路线是否形成一个圆圈，换言之就是判断它是否会移回到原来的位置。<br>
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
给定字符串`J`代表石头中宝石的类型，和字符串`S`代表你拥有的石头。`S`中每个字符代表了一种你拥有的石头的类型，你想知道你拥有的石头中有多少是宝石。<br>
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
最后，从新数组的所有四个方向（即顶部，底部，左侧和右侧）观看的“天际线”必须与原始数组的天际线相同。 城市的天际线是从远处观看时，由所有建筑物形成的矩形的外部轮廓。 请看下面的例子。<br>
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

**说明:**<br>
*   `1 < grid.length = grid[0].length <= 50`。
*   `grid[i][j]` 的高度范围是：`[0, 100]`。
*   一座建筑物占据一个`grid[i][j]`：换言之，它们是`1 x 1 x grid[i][j]`的长方体。<br>

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

## [814. 二叉树剪枝](https://leetcode-cn.com/problems/binary-tree-pruning/description/)

给定二叉树根结点`root`，此外树的每个结点的值要么是`0`，要么是`1`。<br>
返回移除了所有不包含`1`的子树的原二叉树。<br>
( 节点`X`的子树为`X`本身，以及所有`X`的后代。)<br>
**示例1:**
```bash
输入: [1,null,0,0,1]
输出: [1,null,0,null,1]
 
解释: 
只有红色节点满足条件“所有不包含 1 的子树”。
右图为返回的答案。
```
![](images/814_0.png)<br>
**示例2:**
```bash
输入: [1,0,1,0,0,0,1]
输出: [1,null,1,null,1]
```
![](images/814_1.png)<br>
**示例3:**
```bash
输入: [1,1,0,1,1,0,1,0]
输出: [1,1,0,1,1,null,1]
```
![](images/814_2.png)<br>
说明:
给定的二叉树最多有`100`个节点。<br>
每个节点的值只会为`0`或 `1`。<br>

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