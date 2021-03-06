# Binary Search

33.Search in Rotated Sorted Array

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

**Thoughts: check mid point and determine whether target is on the left or right of mid**

> Note: >= is the key here 

```
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        if not nums:
            return -1
        start = 0 
        end = len(nums) - 1
        while start < end: 
            mid = (end + start) / 2
            if nums[mid] == target:
                return mid
            if nums[start] > target > nums[mid] or target > nums[mid] >= nums[start] or nums[mid] >= nums[start] > target:
                start = mid + 1
            else:
                end = mid
        return start if nums[start] == target else -1
```
34.Search for a Range

Given an array of integers sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].

For example,

Given [5, 7, 7, 8, 8, 10] and target value 8,

return [3, 4].

**Thoughts:Findfirst and FindLast**

> Note: Findfirst -> check start point first, FindLast -> check end point first

```
class Solution(object):
    def searchRange(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        if not nums:
            return [-1, -1]
        
        res = [-1, -1]
        if self.searchFirst(nums, target) == -1:
            return res
        else:
            res[0] = self.searchFirst(nums, target)
            res[1] = self.searchLast(nums, target)
            return res
    def searchFirst(self, nums, target):
        start = 0 
        end = len(nums) - 1
        while start + 1 < end:
            mid = start + (end - start)/2
            if nums[mid] >= target:
                end = mid
            else:
                start = mid
        if nums[start] == target:
            return start
        elif nums[end] == target:
            return end
        else:
            return -1
    def searchLast(self, nums, target):
        start = 0 
        end = len(nums) - 1
        while start + 1 < end:
            mid = start + (end - start)/2
            if nums[mid] <= target:
                start = mid
            else:
                end = mid
        if nums[end] == target:
            return end
        elif nums[start] == target:
            return start
        else:
            return -1
```
259.3Sum Smaller

Given an array of n integers nums and a target, find the number of index triplets i, j, k with 0 <= i < j < k < n that satisfy 

the condition nums[i] + nums[j] + nums[k] < target.

Example:

Input: nums = [-2,0,1,3], and target = 2

Output: 2 

Explanation: Because there are two triplets which sums are less than 2:
             [-2,0,1]
             [-2,0,3]
             
**Thoughts: Sort list first, use two pointers at start and at end, find three numbers less than target, 
if found, increase start else decrease end since it is sorted. counts increase end - start**

```
class Solution(object):
    def threeSumSmaller(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        nums[:] = sorted(nums)
        l = len(nums)
        count = 0
        for i in xrange(l):
            left = i + 1
            right = l - 1 
            while left < right:
                if nums[i] + nums[left] + nums[right] < target:
                    count += right - left
                    left += 1
                else:
                    right -= 1
                    
        return count
```



Game of life 



**Thoughts:


249.Group Shifted Strings

Given a string, we can "shift" each of its letter to its successive letter, for example: "abc" -> "bcd". We can keep 

"shifting" which forms the sequence:

"abc" -> "bcd" -> ... -> "xyz"

Given a list of strings which contains only lowercase alphabets, group all strings that belong to the same shifting sequence.

Example:

Input: ["abc", "bcd", "acef", "xyz", "az", "ba", "a", "z"],

Output: 
[
  ["abc","bcd","xyz"],
  ["az","ba"],
  ["acef"],
  ["a","z"]
]
**Thoughts:calcualte off set for each start word to 'a', and minus offset for all chars in word to get same key value for groupings.**

**Special case is when number is less than ord(a), plus 26 **

```
class Solution(object):
    def groupStrings(self, strings):
        """
        :type strings: List[str]
        :rtype: List[List[str]]
        """
        dict = collections.defaultdict(list)
        for s in strings:
            offset = ord(s[0]) - ord('a')
            key = ''
            for i in s:
                k = ord(i) - offset
                if k < ord('a'):
                    k = k + 26
                key += chr(k)
            dict[key].append(s)
        return [items for _, items in dict.items()]
```
406.Queue Reconstruction by Height

Suppose you have a random list of people standing in a queue. Each person is described by a pair of integers (h, k), where h is the height of the person and k is the number of people in front of this person who have a height greater than or equal to h. Write an algorithm to reconstruct the queue.

Note:
The number of people is less than 1,100.


Example

Input:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

Output:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]

**Thoughts: first sort by height and then sort by k, reversely, insert element by k to the final result**

```
class Solution(object):
    def reconstructQueue(self, people):
        """
        :type people: List[List[int]]
        :rtype: List[List[int]]
        """
        if not people:
            return []
        res = []
        people = sorted(people, key=lambda x:(x[0], -x[1]), reverse=True)
        for i in people:
            res.insert(i[1], i)
        return res
```
247.Strobogrammatic Number II

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Find all strobogrammatic numbers that are of length = n.

Example:

Input:  n = 2

Output: ["11","69","88","96"]

T
**Thoughts: check if n is odd or even, start with mid and then append nums for each iteration.**

```
class Solution(object):
    def findStrobogrammatic(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        numbers = ['0', '1', '8'] if n % 2 else ['']
        for _ in range(n//2):
            numbers = [head + s + tail for head, tail in [('0','0'),('1','1'),('6','9'),('8','8'),('9','6')] for s in numbers]
        return [s for s in numbers if s and (s[0]!='0' or len(s)==1)]
                
```

286.Walls and Gates

You are given a m x n 2D grid initialized with these three possible values.

-1 - A wall or an obstacle.
0 - A gate.
INF - Infinity means an empty room. We use the value 231 - 1 = 2147483647 to represent INF as you may assume that the distance to a gate is less than 2147483647.
Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with INF.

Example: 

Given the 2D grid:

INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
After running your function, the 2D grid should be:

  3  -1   0   1
  2   2   1  -1
  1  -1   2  -1
  0  -1   3   4

**Thoughts: start from gate, if rooms[i][j] < dis, then return, else rooms[i][j] = dis**

```
class Solution(object):
    def wallsAndGates(self, rooms):
        """
        :type rooms: List[List[int]]
        :rtype: void Do not return anything, modify rooms in-place instead.
        """
        if not rooms or not rooms[0]:
            return 
        m, n = len(rooms), len(rooms[0])
        for i in xrange(m):
            for j in xrange(n):
                if rooms[i][j] == 0:
                    self.dfs(i, j, rooms, 0)
    def dfs(self, i, j, rooms, dis):
            if i < 0 or j < 0 or i == len(rooms) or j == len(rooms[0]) or rooms[i][j] < dis: return
            rooms[i][j] = dis
            self.dfs(i + 1, j, rooms, dis + 1)
            self.dfs(i - 1, j, rooms, dis + 1)
            self.dfs(i, j + 1, rooms, dis + 1)
            self.dfs(i, j - 1, rooms, dis + 1)
            
```

490.The Maze

There is a ball in a maze with empty spaces and walls. The ball can go through empty spaces by rolling up, down, left or 

right, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.

Given the ball's start position, the destination and the maze, determine whether the ball could stop at the destination.

The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of 

the maze are all walls. The start and destination coordinates are represented by row and column indexes.

**Thoughts: going for one direction until it hits a wall, then change direction**

```
class Solution(object):
    def hasPath(self, maze, start, destination):
        """
        :type maze: List[List[int]]
        :type start: List[int]
        :type destination: List[int]
        :rtype: bool
        """
        pairs = [(1, 0), (-1, 0), (0, 1), (0, -1)]
        self.memo = {}    
        def dfs(maze, pairs, i, j, destination, visited):
            if [i, j] in visited: return False
            if (i, j) in self.memo: return self.memo[(i, j)]
            if [i, j] == destination:return True
            for p in pairs:
                nexti = i + p[0]
                nextj = j + p[1]
                while nexti < len(maze) and nexti >= 0 and nextj < len(maze[0]) and nextj >= 0 and maze[nexti][nextj] == 0:
                    nexti += p[0]
                    nextj += p[1]
                nexti -= p[0]
                nextj -= p[1]
                if dfs(maze, pairs, nexti, nextj, destination, visited + [[i, j]]):
                        self.memo[(i, j)] = True
                        return True
            self.memo[(i, j)] = False
            return False
        return dfs(maze, pairs,start[0], start[1], destination, [])
        
```

418.Sentence Screen Fitting

Given a rows x cols screen and a sentence represented by a list of non-empty words, find how many times the given sentence can be fitted on the screen.

Input:
rows = 2, cols = 8, sentence = ["hello", "world"]

Output: 
1

Explanation:
hello---
world---

The character '-' signifies an empty space on the screen.

**Thoughts: construct sentence first, then fit matrix to sentence see how many cell needed, find position of space. Note: index is one less than len, so when encounter space, we need to plus one. words length is less than the column length.**

```
class Solution(object):
    def wordsTyping(self, sentence, rows, cols):
        """
        :type sentence: List[str]
        :type rows: int
        :type cols: int
        :rtype: int
        """
        s = ' '.join(sentence)+' '
        start, l = 0,len(s)
        for i in xrange(rows):
            start+=cols
            if s[start % l] == ' ':
                start += 1
            else:
                while start >= 0 and s[(start-1)%l] != ' ':
                    start -= 1         
        return start/l
```


417.Pacific Atlantic Water Flow

Given an m x n matrix of non-negative integers representing the height of each unit cell in a continent, the "Pacific ocean" 

touches the left and top edges of the matrix and the "Atlantic ocean" touches the right and bottom edges.

Water can only flow in four directions (up, down, left, or right) from a cell to another one with height equal or lower.

Find the list of grid coordinates where water can flow to both the Pacific and Atlantic ocean.


Example:

Given the following 5x5 matrix:

  Pacific ~   ~   ~   ~   ~ 
       ~  1   2   2   3  (5) *
       ~  3   2   3  (4) (4) *
       ~  2   4  (5)  3   1  *
       ~ (6) (7)  1   4   5  *
       ~ (5)  1   1   2   4  *
          *   *   *   *   * Atlantic

Return:

[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]] (positions with parentheses in above matrix).

**Thoughts: going from top-left and bottom right. divide problem into two separate. find points for atl and pac separate. make sure point is not in the set and 
find intersection**

```
class Solution(object):
    def pacificAtlantic(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[List[int]]
        """
        if not matrix or not matrix[0]:
            return []
        atl, pac, m, n = set(), set(), len(matrix), len(matrix[0])
        def find(i, j, ocean, matrix):
            ocean.add((i, j))
            if i - 1 >= 0 and (i-1, j) not in ocean and matrix[i-1][j] >= matrix[i][j]: find(i-1, j, ocean, matrix)
            if i + 1 < len(matrix) and (i+1, j) not in ocean and matrix[i+1][j] >= matrix[i][j]: find(i+1, j, ocean, matrix)
            if j - 1 >= 0 and (i, j-1) not in ocean and matrix[i][j-1] >= matrix[i][j]: find(i, j-1, ocean, matrix)
            if j + 1 < len(matrix[0]) and (i, j+1) not in ocean and matrix[i][j+1] >= matrix[i][j]: find(i, j+1, ocean, matrix)
        for x in xrange(max(m, n)):
            if x < n and (0, x) not in pac: find(0, x, pac, matrix)
            if x < m and (x, 0) not in pac: find(x, 0, pac, matrix)
            if x < m and (x, n-1) not in atl: find(x, n-1, atl, matrix)
            if x < n and (m-1, x) not in atl: find(m-1, x, atl, matrix)
        return [[x, y] for x, y in atl&pac]
```

325.Maximum Size Subarray Sum Equals k

Given an array nums and a target value k, find the maximum length of a subarray that sums to k. If there isn't one, return 0 instead.

Note:

The sum of the entire nums array is guaranteed to fit within the 32-bit signed integer range.

Example 1:

Given nums = [1, -1, 5, -2, 3], k = 3,

return 4. (because the subarray [1, -1, 5, -2] sums to 3 and is the longest)

**Thoughts: make sure 0 is at the start points [-1, 1] k=0 . i - (sums - k) is the length of the array**

```
class Solution(object):
    def maxSubArrayLen(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        sums = 0
        maxl = 0
        maps = {0:-1}
        for i in xrange(len(nums)):
            sums += nums[i]
            if sums not in maps:
                maps[sums] = i
            if sums - k in maps:
                maxl = max(maxl, i - maps[sums-k])
        return maxl
```

314.Binary Tree Vertical Order Traversal

Given a binary tree, return the vertical order traversal of its nodes' values. (ie, from top to bottom, column by column).

If two nodes are in the same row and column, the order should be from left to right.

Given binary tree [3,9,20,null,null,15,7],
   3
  /\
 /  \
 9  20
    /\
   /  \
  15   7
return its vertical order traversal as:
[
  [9],
  [3,15],
  [20],
  [7]
]

**Thoughts: BFS, left node -1 and right node +1 to keep track of levels and also keep track of min and max levels**

> Note: min and max are None in the end, so we need to +/- 1

```
class Solution(object):
    def verticalOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        min_col = max_col = 0
        deque = collections.deque([(root, 0)])
        dicts = collections.defaultdict(list)
        res = []
        while deque:
            cur, col = deque.popleft()
            min_col = min(col, min_col)
            max_col = max(col, max_col)
            if cur:
                dicts[col].append(cur.val)
                deque.append((cur.left, col+1))
                deque.append((cur.right, col-1))
        # min_col and max_col are None, so we need to +/- 1
        for i in reversed(range(min_col+1, max_col)):
            res.append(dicts[i])
        return res
```

545. Boundary of Binary Tree

Given a binary tree, return the values of its boundary in anti-clockwise direction starting from root. Boundary includes left boundary, leaves, and right boundary in order without duplicate nodes.

Left boundary is defined as the path from root to the left-most node. Right boundary is defined as the path from root to the right-most node. If the root doesn't have left subtree or right subtree, then the root itself is left boundary or right boundary. Note this definition only applies to the input binary tree, and not applies to any subtrees.

The left-most node is defined as a leaf node you could reach when you always firstly travel to the left subtree if exists. If not, travel to the right subtree. Repeat until you reach a leaf node.

The right-most node is also defined by the same way with left and right exchanged.

Example 1
Input:
  1
   \
    2
   / \
  3   4

Ouput:
[1, 3, 4, 2]

Explanation:
The root doesn't have left subtree, so the root itself is left boundary.
The leaves are node 3 and 4.
The right boundary are node 1,2,4. Note the anti-clockwise direction means you should output reversed right boundary.
So order them in anti-clockwise without duplicates and we have [1,3,4,2].

**Thoughts: from left boundary + leaves + right boundary. pass root.left and root.right to left b and right b respectively**

```
class Solution(object):
    def boundaryOfBinaryTree(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if not root: return []
        res = []
        res.append(root.val)
        self.left_side(root.left, res)
        self.leaf(root.left, res)
        self.leaf(root.right, res)
        self.right_side(root.right, res)
        return res
        
    def left_side(self, root, res):
        if not root or (not root.left and not root.right): return
        res.append(root.val)
        if root.left: self.left_side(root.left, res)
        else: self.left_side(root.right, res)
        
    
    def right_side(self, root, res):
        if not root or (not root.left and not root.right): return
        if root.right: self.right_side(root.right, res)
        else: self.right_side(root.left, res)
        res.append(root.val)
    
    def leaf(self, root, res):
        if not root: return
        if not root.left and not root.right: 
            res.append(root.val)
            return
        self.leaf(root.left, res)
        self.leaf(root.right, res)
```

763.Partition Labels

A string S of lowercase letters is given. We want to partition this string into as many parts as possible so that each letter appears in at most one part, and return a list of integers representing the size of these parts.

Example 1:
Input: S = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits S into less parts.

**Thoughts: create a index, and check intersection between two parts divided by index**

```
class Solution(object):
    def partitionLabels(self, S):
        """
        :type S: str
        :rtype: List[int]
        """
        res = []
        while S:
            i = 1
            while set(S[:i]) & set(S[i:]):
                i += 1
            res.append(i)
            S = S[i:]
        return res
```

671.Second Minimum Node In a Binary Tree

Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly two or zero sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes.

Given such a binary tree, you need to output the second minimum value in the set made of all the nodes' value in the whole tree.

If no such second minimum value exists, output -1 instead.

**Thoughts: traverse tree only with node smaller than the current second smallest value**

```
class Solution(object):
    def findSecondMinimumValue(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        res = [float('inf')]
        def traverse(node):
            if not node:
                return
            if root.val < node.val < res[0]:
                res[0] = node.val
            if node.left and node.left.val < res[0]: traverse(node.left)        
            if node.right and node.right.val < res[0]: traverse(node.right)
        traverse(root)
        return -1 if res[0] == float('inf') else res[0]
```
