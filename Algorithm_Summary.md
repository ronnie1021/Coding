# Bit manipulation
338.Counting Bits

Given a non negative integer number num. For every numbers i in the range 0 ≤ i ≤ num calculate the number of 1's in their binary representation and return them as an array.

Example:

For num = 5 you should return [0,1,1,2,1,2].

Follow up:

It is very easy to come up with a solution with run time O(n*sizeof(integer)). But can you do it in linear time O(n) /possibly in a single pass?

Space complexity should be O(n).

Can you do it like a boss? Do it without using any builtin function like __builtin_popcount in c++ or in any other language.

**Thoughts: Tricks, nums of 1 in i is nums of 1 in i&i-1 + 1  

```
class Solution(object):
    def countBits(self, num):
        """
        :type num: int
        :rtype: List[int]
        """
        res = [0]*(num+1)
        for i in range(1,num+1):
            res[i] = res[i&(i-1)] + 1
        return res
```

# LinkedList

##You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.
You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Thoughts are iterate through if l1, l2, or carry exists to next node** 

```
class Solution(object):
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        cur = dummy = ListNode(0)
        carry = 0
        while l1 or l2 or carry:
            if l1:
                carry += l1.val
                l1 = l1.next
            if l2:
                carry += l2.val
                l2 = l2.next
            cur.next = ListNode(carry%10)
            cur = cur.next
            carry = carry // 10
        return dummy.next
```    
328. Odd Even Linked List

Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.

Example:

Given 1->2->3->4->5->NULL,

return 1->3->5->2->4->NULL.

**Thoughts: check if head.next is null, and just iterate throgh 

```
class Solution(object):
    def oddEvenList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        odd = dummy = ListNode(0) 
        even = dummy1 = ListNode(0)
        while head:
            odd.next = head
            even.next = head.next
            even = head.next
            odd = head
            head = even.next if even else None
        odd.next = dummy1.next
        return dummy.next
```
138.Copy List with Random Pointer

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

**Thoughts: DeepCopy: mean create separate memory location then original one, so we have to use collections.defaultdict()**

> Note: Collections.defaultdict() will not give an error when undefined keys are called

> Specify None in the dict. and use key values to reference original nodes 

```
class Solution(object):
    def copyRandomList(self, head):
        """
        :type head: RandomListNode
        :rtype: RandomListNode
        """
        dic = collections.defaultdict(lambda: RandomListNode(0))
        dic[None] = None
        dummy = head
        while head:
            dic[head].label = head.label
            dic[head].next = dic[head.next]
            dic[head].random = dic[head.random]
            head = head.next
        return dic[dummy]
```


5.Longest Palindromic Substring

Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

**Thoughts: iterate through all characters, treat them as mid point and search left and right**

```
class Solution(object):
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        r = ''
        if len(s) == 1 or not s or s[::-1] == s:
            return s
        def helper(s, i , j):
            while i >=0 and j <= len(s) -1:
                if s[i] == s[j]:
                    i -= 1
                    j += 1
                else:
                    break
            return s[i+1:j]
        for i in xrange(len(s)):
            cur = helper(s, i , i)
            if len(cur) > len(r):
                r = cur
            if s[i] != len(s) -1:
                cur = helper(s, i, i+1)
                if len(cur) > len(r):
                    r = cur                 
        return r
```
647.Palindromic Substrings

Given a string, your task is to count how many palindromic substrings in this string.

The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.

**Thoughts: Same as longest palindromic substring

```
class Solution(object):
    def countSubstrings(self, s):
        """
        :type s: str
        :rtype: int
        """
        self.count = 0
        def helper(s, i, j):
            while i >= 0 and j <= len(s) - 1 and (s[i] == s[j]):
                self.count += 1
                i -= 1
                j+=1
        for i in xrange(len(s)):
            helper(s, i, i)
            if i != len(s) - 1:
                helper(s, i, i+1)
        return self.count
```


# DP 

3.Longest Substring without repeating characters

Given a string, find the length of the longest substring without repeating characters.

Examples:

Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.

**Thoughts: Find position of duplicated character and discard everything before**

```
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        cur = r = ''
        for i in xrange(len(s)):
            if s[i] not in cur:
                cur += s[i]
            else:
                cur = cur[cur.index(s[i]) + 1:] + s[i]
            if len(cur) > len(r):
                r = cur  
        return len(r)
```

746.Min Cost Climbing Stairs
On a staircase, the i-th step has some non-negative cost cost[i] assigned (0 indexed).

Once you pay the cost, you can either climb one or two steps. You need to find minimum cost to reach the top of the floor, and you can either start from the step with index 0, or the step with index 1.

Example 1:
Input: cost = [10, 15, 20]

Output: 15
Explanation: Cheapest is start on cost[1], pay that cost and go to the top.

Example 2:
Input: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]

Output: 6
Explanation: Cheapest is start on cost[0], and only step on 1s, skipping cost[3].

Note:
cost will have a length in the range [2, 1000].
Every cost[i] will be an integer in the range [0, 999].

**Thoughts: for every step, we record the maximum value for both one step cost and two step cost**

```
class Solution(object):
    def minCostClimbingStairs(self, cost):
        """
        :type cost: List[int]
        :rtype: int
        """
        if not cost:
            return 0
        if len(cost) <= 2:
            return min(cost)
        two_step_away = cost[0]
        one_step_away = cost[1]
        for cost in cost[2:]:
            one_step_away, two_step_away = min(two_step_away, one_step_away) + cost , one_step_away
        return min(one_step_away, two_step_away)
```
53.Maximum Subarray

Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array [-2,1,-3,4,-1,2,1,-5,4],

the contiguous subarray [4,-1,2,1] has the largest sum = 6

**Thoughts:maximum product, you have to keep record of maximum value and current value combination every step when iterate through**

```
class Solution(object):
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
        maxv = cur = float('-inf')
        for i in nums:
            cur = max(cur + i, i)
            maxv = max(cur, maxv)
        return maxv
```
91.Decode Ways
A message containing letters from A-Z is being encoded to numbers using the following mapping:

'A' -> 1
'B' -> 2
...
'Z' -> 26

Given an encoded message containing digits, determine the total number of ways to decode it.

For example,
Given encoded message "12", it could be decoded as "AB" (1 2) or "L" (12).

The number of ways decoding "12" is 2.

**Thoughts: Conditional dp problem, have to check whether number and combined with previous number within range when iterate through**

**There are hidden dp transition formulas for each different conditions**
> Remember to return 0 when no condition is satisfied

```
class Solution(object):
    def numDecodings(self, s):
        """
        :type s: str
        :rtype: int
        """
        if not s or s[0] == '0':
            return 0
        res = [0]*(len(s))
        res[0] = 1 
        res[-1] = 1
        for i in range(1, len(s)):
            if s[i] != '0' and int(s[i-1]+s[i]) <= 26 and int(s[i-1]+s[i]) >= 10:
                res[i] = res[i-1] + res[i-2]
            elif s[i] != '0':
                res[i] = res[i-1]
            elif int(s[i-1]+s[i]) <= 26 and int(s[i-1]+s[i]) >= 10:
                res[i] = res[i-2]
            else:
                return 0
        return res[len(s)-1]
```

152.Maximum Product Subarray

Find the contiguous subarray within an array (containing at least one number) which has the largest product.

For example, given the array [2,3,-2,4],

the contiguous subarray [2,3] has the largest product = 6.

**Thoughts: keep track of minimum, maximum through each iteration**

> Note:  Update current max and current min before compare with global max

```
class Solution(object):
    def maxProduct(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        gmax = maxv = minv = nums[0]    
        for i in nums[1:]:
            maxv, minv = max(maxv*i, minv*i, i), min(maxv*i, minv*i, i)
            gmax = max(gmax, maxv)
        return gmax
```

309.Best Time to Buy and Sell Stock with Cooldown

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the 

stock multiple times) with the following restrictions:

You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)

Example:

- prices = [1, 2, 3, 0, 2]
- maxProfit = 3
- transactions = [buy, sell, cooldown, buy, sell]

**Thoughts. there are three states: buy, sell, cool at each time. if you want to buy at a particular time i, you could either cool or buy at i - 1 and if you want to sell at time i, at time i-1 the only thing you can do is buy, you want to cool at time i, you can either
cool or sell at time i - 1
# Backtracking
17.Letter Combinations of a Phone Number

Given a digit string, return all possible letter combinations that the number could represent.


A mapping of digit to letters (just like on the telephone buttons) is given below.

**Thoughts: backtracking, keep tracks of latest combinations when iterate through digits** 

```
class Solution(object):
    def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """
        letter_map = {'2':'abc', '3':'def', '4':'ghi', '5':'jkl', '6':'mno',                   '7':'pqrs', '8':'tuv', '9':'wxyz'}
        if not digits:
            return []
        res = list(letter_map[digits[0]])
        for i in digits[1:]:
            res = [r + j for r in res for j in letter_map[i]]
        return res  
```

89.Gray Code

The gray code is a binary numeral system where two successive values differ in only one bit.

Given a non-negative integer n representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.

For example, given n = 2, return [0,1,3,2]. Its gray code sequence is:

00 - 0
01 - 1
11 - 3
10 - 2

**Thoughts: Backtrack, every iteration, adding from back to front.**

```
class Solution(object):
    def grayCode(self, n):
        """
        :type n: int
        :rtype: List[int]
        """
        res = [0]
        for i in range(0, n):
            res += [j + 2**i for j in res[::-1]]
        return res
```

254.Factor Combinations
Numbers can be regarded as product of its factors. For example,

8 = 2 x 2 x 2;
  = 2 x 4.

Write a function that takes an integer n and return all possible combinations of its factors.

> Note: </br>
You may assume that n is always positive. </br>
Factors should be greater than 1 and less than n.

**Thoughts: find each num, it has to go through ever combination with different factors, and recursively for its num/div  

```
class Solution(object):
    def getFactors(self, n):
        """
        :type n: int
        :rtype: List[List[int]]
        """        
        to_do, ans = [([], 2, n)], []
        while to_do:
            factor, div, num = to_do.pop()
            while div**2 <= num:
                if num%div == 0:
                    ans.append([div, num/div] + factor)
                    to_do += [(factor+[div], div, num/div)]
                div += 1
        return ans
```

# Two pointers

11.Container With Most Water
Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2

**Thoughts: two pointers, start and end, move those pointers towards each other. move whichever is less (start or end)**

```
class Solution(object):
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        start = 0 
        end = len(height) - 1
        area = 0
        while start < end:
            area  = max(area, (end - start )*min(height[start], height[end]))
            if height[start] < height[end]:
                start += 1
            else:
                end -= 1
        return area
```

