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
