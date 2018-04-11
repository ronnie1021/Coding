# DFS
19.Remove Nth Node From End of List

Given a linked list, remove the nth node from the end of list and return its head.

Note:
Given n will always be valid.
Try to do this in one pass.

**Thoughts: Recusive, find nth node from end and remove it**

```
class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        self.count = 1
        def dfs(head, n):
            if not head:
                return head
            head.next = dfs(head.next, n)
            if self.count == n:
                self.count += 1
                return head.next
            self.count += 1
            return head
        return dfs(head, n)
```

## BFS

```
