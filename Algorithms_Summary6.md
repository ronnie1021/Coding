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


117.Populating Next Right Pointers in Each Node II

```
# Definition for binary tree with next pointer.
# class TreeLinkNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
#         self.next = None

class Solution:
    # @param root, a tree link node
    # @return nothing
    def connect(self, node):
        tail = dummy = TreeLinkNode(0)
        while node:
            tail.next = node.left
            if tail.next:
                tail = tail.next
            tail.next = node.right
            if tail.next:
                tail = tail.next
            node = node.next
            if not node:
                tail = dummy
                node = dummy.next
```
299.Bulls and cows

Example 1:

Input: secret = "1807", guess = "7810"

Output: "1A3B"

Explanation: 1 bull and 3 cows. The bull is 8, the cows are 0, 1 and 7.

Example 2:

Input: secret = "1123", guess = "0111"

Output: "1A1B"

Explanation: The 1st 1 in friend's guess is a bull, the 2nd or 3rd 1 is a cow.

```
class Solution(object):
    def getHint(self, secret, guess):
        """
        :type secret: str
        :type guess: str
        :rtype: str
        """
        index = A = 0
        bulls = []
        while index < len(secret):
            if secret[index] == guess[index]:
                A += 1
            index += 1
        total = sum([min(secret.count(x), guess.count(x)) for x in set(secret)]) 
            
            
        return str(A)+'A'+str(total-A)+'B'
                
```

156.Binary Tree Upside Down

Given a binary tree where all the right nodes are either leaf nodes with a sibling (a left node that shares the same parent node) or empty, flip it upside down and turn it into a tree where the original right nodes turned into left leaf nodes. Return the new root.

Example:

Input: [1,2,3,4,5]

    1
   / \
  2   3
 / \
4   5

Output: return the root of the binary tree [4,5,2,#,#,3,1]

   4
  / \
 5   2
    / \
   3   1  

```
class Solution(object):
    def upsideDownBinaryTree(self, root):
        """
        :type root: TreeNode
        :rtype: TreeNode
        """
        
        def dfs(root):
            if not root or (not root.left and not root.right): return root
            newroot = dfs(root.left)
            root.left.left = root.right
            root.left.right = root
            root.left = None
            root.right = None
            return newroot
        
        return dfs(root)
```



43.Multiply Strings

Given two non-negative integers num1 and num2 represented as strings, return the product of num1 and num2, also represented as a string.

```
class Solution(object):
    def multiply(self, num1, num2):
        """
        :type num1: str
        :type num2: str
        :rtype: str
        """
        l1, l2 = len(num1), len(num2)
        result = [0]*(l1+l2)
        for i in reversed(range(l1)):
            for j in reversed(range(l2)):
                index1 = i + j
                index2 = i + j + 1
                mp = int(num1[i]) * int(num2[j])
                sums = mp + result[index2]
                result[index2] += (sums % 10)
                result[index1] += (sums // 10)
        return str(int(''.join(map(str, result))))
                
```

24.Swap Nodes in Pairs

Given 1->2->3->4, you should return the list as 2->1->4->3.

```
class Solution(object):
    def swapPairs(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        pre, pre.next = self, head
        while pre.next and pre.next.next:
            a = pre.next
            b = a.next
            pre.next, b.next, a.next= b, a, b.next
            pre = a
        return self.next
    
```
311.Sparse Matrix Multiplication

Given two sparse matrices A and B, return the result of AB.

You may assume that A's column number is equal to B's row number.

```
class Solution(object):
    def multiply(self, A, B):
        """
        :type A: List[List[int]]
        :type B: List[List[int]]
        :rtype: List[List[int]]
        """
        m1, m2 = len(A), len(A[0])
        n1, n2 = len(B), len(B[0])
        result = [n2*[0] for i in range(m1)]
        for i in range(m1):
            for j in range(m2):
                if A[i][j]:
                    for t in range(n2):
                        if B[j][t]:
                            result[i][t] += A[i][j]*B[j][t]
        return result                
```
