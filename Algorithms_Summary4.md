205.Isomorphic Strings

Given two strings s and t, determine if they are isomorphic.

Two strings are isomorphic if the characters in s can be replaced to get t.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

Example 1:

Input: s = "egg", t = "add"

Output: true

**Thoughts:use index to record repeat pattern**

```
class Solution(object):
    def isIsomorphic(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        dict1 = collections.defaultdict(list)
        dict2 = collections.defaultdict(list) 
        for index, val in enumerate(s):
            dict1[val].append(index)
        for index, val in enumerate(t):
            dict2[val].append(index)
        return sorted(dict1.values()) == sorted(dict2.values())
```



616.Add Bold Tag in String

Given a string s and a list of strings dict, you need to add a closed pair of bold tag <b> and </b> to wrap the substrings in 

s that exist in dict. If two such substrings overlap, you need to wrap them together by only one pair of closed bold tag. 

Also, if two substrings wrapped by bold tags are consecutive, you need to combine them.

**Thoughts: find index of bold string, and add index to the set. brutal force search**
```
class Solution(object):
    def addBoldTag(self, s, dict):
        """
        :type s: str
        :type dict: List[str]
        :rtype: str
        """
        bold = set()
        for w in dict:
            l = len(w)
            for i in xrange(len(s)):
                if s[i:i+l] == w:
                    for j in range(i, i+l):
                        bold.add(j)
                
        res = ''
        for i in range(len(s)):
            if i in bold and i-1 not in bold: res += '<b>'
            res += s[i]
            if i in bold and i+1 not in bold: res += '</b>'
        return res
```

186.Reverse Words in a String II

Given an input string , reverse the string word by word. 

Example:

Input:  ["t","h","e"," ","s","k","y"," ","i","s"," ","b","l","u","e"]
Output: ["b","l","u","e"," ","i","s"," ","s","k","y"," ","t","h","e"]
Note: 

A word is defined as a sequence of non-space characters.

The input string does not contain leading or trailing spaces.

The words are always separated by a single space.

Follow up: Could you do it in-place without allocating extra space?

**Thoughts: First reverse the entire string, then reverse each of them. careful on edges**

```
class Solution(object):
    def reverseWords(self, str):
        """
        :type str: List[str]
        :rtype: void Do not return anything, modify str in-place instead.
        """
        if not str:
            return 
        def reverse(s, l, r):
            for i in range((r - l) // 2):
                s[l+i], s[r - 1 - i] = s[r - 1 - i], s[l+i]
        reverse(str, 0, len(str))
        
        index = 0
        for i in xrange(len(str) + 1):
            if i == len(str) or str[i] == " ":
                reverse(str, index, i)
                index = i + 1
                
```

438.Find All Anagrams in a String

Given a string s and a non-empty string p, find all the start indices of p's anagrams in s.

Strings consists of lowercase English letters only and the length of both strings s and p will not be larger than 20,100.

The order of output does not matter.

Example 1:

Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".

The substring with start index = 6 is "bac", which is an anagram of "abc".

**Thoughts: Maintain a sliding windows equal to len(p), put index when count = 0, otherwise keep moving **

```
class Solution(object):
    def findAnagrams(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: List[int]
        """
        if len(s) < len(p) or not s or not p:
            return []
        counter = collections.Counter(p)
        count = n = len(p)
        left = right= 0
        res = []
        while right < len(s):
            if counter[s[right]] > 0:
                count -= 1
            counter[s[right]] -= 1
            
            if count == 0:
                res.append(left)
                
            if (n - 1) == (right - left):
                counter[s[left]] += 1 
                if counter[s[left]] > 0:
                    count += 1
                left += 1
            right += 1
        return res
```
163.Missing Ranges

Given a sorted integer array nums, where the range of elements are in the inclusive range [lower, upper], return its missing 

ranges.

Example:

Input: nums = [0, 1, 3, 50, 75], lower = 0 and upper = 99,
Output: ["2", "4->49", "51->74", "76->99"]

**Thoughts: Combine lower and upper to nums array**

```
class Solution(object):
    def findMissingRanges(self, nums, lower, upper):
        """
        :type nums: List[int]
        :type lower: int
        :type upper: int
        :rtype: List[str]
        """
        res = []
        nums = [lower-1] + nums + [upper+1]
        for i in range(1, len(nums)):
            prev = nums[i-1]
            if nums[i] - prev > 1:
                low = prev + 1
                high = nums[i] - 1
                if low == high: res.append(str(prev+1))
                else: res.append(str(prev+1) + '->' + str(nums[i]- 1))   
        return res
```

289.Game of Life

According to the Wikipedia's article: "The Game of Life, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

Given a board with m by n cells, each cell has an initial state live (1) or dead (0). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

Any live cell with fewer than two live neighbors dies, as if caused by under-population.
Any live cell with two or three live neighbors lives on to the next generation.
Any live cell with more than three live neighbors dies, as if by over-population..
Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.
Write a function to compute the next state (after one update) of the board given its current state.

Follow up: 
Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.
In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?

**Thoughts: inplace, 2 is key here **

```
class Solution(object):
    def gameOfLife(self, board):
        if not board or not board[0]:
            return
        def find(i, j):
            count =0
            for x, y in [(i+1, j+1), (i-1, j-1), (i-1, j), (i, j-1), (i+1, j), (i, j+1), (i+1, j-1), (i-1, j+1)]:
                try:
                    if x > -1 and y > -1 and board[x][y]%2:
                        count += 1
                except:
                    pass
            return count
        
        m, n = len(board), len(board[0])
        for i in xrange(m):
            for j in xrange(n):
                count = find(i, j)
                if not board[i][j] % 2 and count == 3:
                    board[i][j] += 2
                elif board[i][j] % 2 and (count == 3 or count == 2):
                    board[i][j] += 2
        for i in xrange(m):
            for j in xrange(n):
                board[i][j] = board[i][j] / 2
```

253.Meeting Rooms II

Given an array of meeting time intervals consisting of start and end times [[s1,e1],[s2,e2],...] (si < ei), find the minimum 

number of conference rooms required.

Example 1:

Input: [[0, 30],[5, 10],[15, 20]]
Output: 2
Example 2:

Input: [[7,10],[2,4]]
Output: 1

**Thoughts: sort start time and end time separately, go through sorted start time, if start < end, count +=1 else end += 1**

```
class Solution(object):
    def minMeetingRooms(self, intervals):
        """
        :type intervals: List[Interval]
        :rtype: int
        """
        starts = []
        ends = []
        for i in intervals:
            starts.append(i.start)
            ends.append(i.end)
        starts = sorted(starts)
        ends = sorted(ends)
        end = 0
        count = 0
        for i in xrange(len(starts)):
            if starts[i] < ends[end]:
                count += 1
            else:
                end += 1
        return count
```

# Divide and conquer

241.Different Ways to Add Parentheses

Given a string of numbers and operators, return all possible results from computing all the different possible ways to group 

numbers and operators. The valid operators are +, - and *.

Input: "2-1-1"
Output: [0, 2]
Explanation: 
((2-1)-1) = 0 
(2-(1-1)) = 2


**Thoughts: Divide it based on operators, until only one integer on each side, run through all combinations of results**


```
class Solution(object):
    def diffWaysToCompute(self, input):
        """
        :type input: str
        :rtype: List[int]
        """
        if input.isdigit():
            return [int(input)]
        res = []        
        for i in xrange(len(input)):
            if input[i] in "-+*":
                res1 = self.diffWaysToCompute(input[:i])
                res2 = self.diffWaysToCompute(input[i+1:])
                res += [eval(str(k)+input[i]+str(j)) for k in res1 for j in res2]            
        return res
```

18.4Sum

Given an array nums of n integers and an integer target, are there elements a, b, c, and d in nums such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.


**Thoughts: Sort list and divide problem into two sum, check if num is equal previous one, if so continue to avoid duplicates **

```
class Solution(object):
    def fourSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[List[int]]
        """  
        self.r = []
        def findNsum(nums, N, target, res):
            if len(nums) < N or N < 2 or nums[0]*N > target or nums[-1]*N < target: return 
            if N == 2: 
                l,r = 0,len(nums)-1
                while l < r:
                    s = nums[l] + nums[r]
                    if s == target:
                        self.r.append(res + [nums[l], nums[r]])
                        l += 1
                        while l < r and nums[l] == nums[l-1]:
                            l += 1
                    elif s < target:
                        l += 1
                    else:
                        r -= 1
            else:
                for i in range(len(nums)-N+1):
                    if i == 0 or (nums[i] != nums[i-1]):
                        findNsum(nums[i+1:], N-1, target-nums[i], res+[nums[i]])
        findNsum(sorted(nums), 4, target, [])
        return self.r
```
