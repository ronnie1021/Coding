
# Iterate Through
## LinkedList

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

# DP and backtracking

3. Longest Substring without repeating characters
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
5. Longest Palindromic Substring
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
