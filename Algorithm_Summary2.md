# DFS
19.Remove Nth Node From End of List

Given a linked list, remove the nth node from the end of list and return its head.

Note:
Given n will always be valid.
Try to do this in one pass.

**Thoughts: Recusive, find nth node from end and remove it**

```
class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        self.count = 1
        def dfs(head, n):
            if not head:
                return head
            head.next = dfs(head.next, n)
            if self.count == n:
                self.count += 1
                return head.next
            self.count += 1
            return head
        return dfs(head, n)
```
669.Trim a Binary Search Tree

Given a binary search tree and the lowest and highest boundaries as L and R, trim the tree so that all its elements lies in [L, R] (R >= L). You might need to change the root of the tree, so the result should return the new root of the trimmed binary search tree.

**Thoughts: check root value, if not in range, then it must mean at least left or right not in range. return its left or right **

```
class Solution(object):
    def trimBST(self, root, L, R):
        """
        :type root: TreeNode
        :type L: int
        :type R: int
        :rtype: TreeNode
        """
        if not root:
            return None
        root.left = self.trimBST(root.left, L, R)
        root.right = self.trimBST(root.right, L, R) 
        
        if root.val <= R and root.val >= L:
            return root
        return root.left or root.right
```

654.Maximum Binary Tree

Given an integer array with no duplicates. A maximum tree building on this array is defined as follow:

The root is the maximum number in the array.

The left subtree is the maximum tree constructed from left part subarray divided by the maximum number.

The right subtree is the maximum tree constructed from right part subarray divided by the maximum number.

Construct the maximum tree by the given array and output the root node of this tree.

**Thoughts: find max number in array and divide array at each recursion step**

```
class Solution(object):
    def constructMaximumBinaryTree(self, nums):
        """
        :type nums: List[int]
        :rtype: TreeNode
        """
        if not nums:
            return None
        ind = nums.index(max(nums))
        root = TreeNode(max(nums))
        root.left = self.constructMaximumBinaryTree(nums[:ind])
        root.right = self.constructMaximumBinaryTree(nums[ind+1:])
        return root
```
617.Merge Two Binary Trees

Given two binary trees and imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while 

the others are not.

You need to merge them into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of 

the merged node. Otherwise, the NOT null node will be used as the node of new tree.

**Thoughts:if t1 or t2 is None, return one that is not None, no need to combine**

```
class Solution(object):
    def mergeTrees(self, t1, t2):
        """
        :type t1: TreeNode
        :type t2: TreeNode
        :rtype: TreeNode
        """
        if not t1 or not t2:
            return t1 or t2
        root = TreeNode(t1.val+t2.val)
        root.left = self.mergeTrees(t1.left, t2.left)
        root.right = self.mergeTrees(t1.right, t2.right)
        return root
```
366.Find Leaves of Binary Tree

Given a binary tree, collect a tree's nodes as if you were doing this: Collect and remove all leaves, repeat until the tree is empty.

**Thoughts: Have to know this is post-order traversalm find all leaves first. you have to count which level is it on.**

```
class Solution(object):
    def findLeaves(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
        self.ans = collections.defaultdict(list)
        r = []
        def dfs(root, ans):
            if not root:
                return 0  
            left = dfs(root.left, ans)
            right = dfs(root.right, ans)
            level = max(left, right)
            ans[level] += [root.val]
            return level + 1
        dfs(root, self.ans)
        for i in sorted(self.ans.keys()):
            r.append(self.ans[i])
        return r
```
200.Number of Islands

Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by 

connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Thoughts: Iterate through grid, for each element equals 1, convert every nearby 1 to 0 recursively. convert from 1 to 0 before calling recursive function**

```
class Solution(object):
    def numIslands(self, grid):
        """
        :type grid: List[List[str]]
        :rtype: int
        """
        if not grid:
            return 0
        count = 0
        for i in xrange(len(grid)):
            for j in xrange(len(grid[i])):
                if self.sink(grid, i, j):
                    count += 1
        return count
    def sink(self, grid, i, j):
        if grid[i][j] == '1':
            grid[i][j] = 0 
            if i < len(grid) - 1 and i >= 0:
                self.sink(grid, i+1, j)
            if i <= len(grid) - 1 and i > 0:
                self.sink(grid, i-1, j)
            if j >= 0 and j < len(grid[i]) - 1:
                self.sink(grid, i, j+1)
            if j > 0 and j <= len(grid[i]) - 1:
                self.sink(grid, i, j-1)
            return True
```
22. Generate Parentheses

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]

**Thoughts, left parentheses always go first, next check if left < right or not**

```
class Solution(object):
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        if not n:
            return []
        self.res = []
        sets = ''
        def dfs(left, right, sets):
            if not left and not right:
                self.res.append(sets)
            if left:
                dfs(left - 1, right, sets+'(')
            if left < right:
                dfs(left, right - 1, sets+')')
        dfs(n,n,sets)
        return self.res
```
236.Lowest Common Ancestor of a Binary Tree

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow a node to be a descendant of itself).”

**Thoughts: make nodes that are not given two nodes null, pass two nodes above till left and right not null **

```
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if root == p or root == q or root == None:
            return root    
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if left and right:
            return root
        return left or right
```




## BFS

515.Find Largest Value in Each Tree Row

You need to find the largest value in each row of a binary tree.

**Thoughts: BFS, go through every row**

```
class Solution(object):
    def largestValues(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if not root:
            return []
        queue = [root]
        ans = []
        while queue:
            ans += [max([q.val for q in queue])]
            queue = [i for q in queue for i in (q.left,q.right) if i]
        return ans
```

279.Perfect Squares

Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.


For example, given n = 12, return 3 because 12 = 4 + 4 + 4; given n = 13, return 2 because 13 = 4 + 9

**Thoughts: find root of n, set up candidates list, iterate through every combination**

```
class Solution(object):
    def numSquares(self, n):
        """
        :type n: int
        :rtype: int
        """
        root = int(math.sqrt(n))
        candidates = [i**2 for i in range(1, root+1)]
        count = 1
        res = [n]
        while res:
            for i in res:
                    if i in candidates: return count
            res = [i-j for i in res for j in candidates if i > j]
            count += 1
        return 0
```

127.Word Ladder

Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from 

beginWord to endWord, such that:

Only one letter can be changed at a time.

Each transformed word must exist in the word list. Note that beginWord is not a transformed word.

For example,

Given:

beginWord = "hit"

endWord = "cog"

wordList = ["hot","dot","dog","lot","log","cog"]

As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",

return its length 5.

**Thoughts: each iteration change one char at each position of begin word **

> Note: check whether newlist is null not WordList

```
class Solution(object):
    def ladderLength(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: List[str]
        :rtype: int
        """
        if not wordList or not beginWord or not endWord:
            return 0
        count = 2  
        wordList = set(wordList)
        charset = {c for i in wordList for c in i}
        newList = {beginWord}
        endWord = {endWord}
        while newList:
            newList = wordList & {word[:c]+ char + word[c+1:] for word in newList for c in xrange(len(word)) for char in 
            charset if word[c] != char}
            if endWord & newList:
                return count
            else:
                wordList = wordList - newList
                count+=1
        return 0
```
