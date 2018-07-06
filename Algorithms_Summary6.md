394.Decode String

Given an encoded string, return it's decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like 3a or 2[4].

Examples:

s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".

```
class Solution(object):
    def decodeString(self, s):
        """
        :type s: str
        :rtype: str
        """
        stack = []
        start = 0
        cur  = ''
        cur_num = 0
        while start < len(s):
            if s[start] == ']':
                strs = stack.pop()
                n = int(stack.pop())
                cur = strs + n * cur  
                cur_num = 0
            elif s[start].isdigit():
                cur_num = cur_num*10 + int(s[start])     
            elif s[start] == '[':
                stack.append(cur_num)
                stack.append(cur)
                cur = ''
                cur_num = 0
            else:
                cur += s[start]
            start += 1
        return cur
```
