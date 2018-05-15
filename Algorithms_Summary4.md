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
