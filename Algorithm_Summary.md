
# Iterate Through

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

