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
