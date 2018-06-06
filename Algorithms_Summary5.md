386.Lexicographical Numbers

Given an integer n, return 1 - n in lexicographical order.

For example, given 13, return: [1,10,11,12,13,2,3,4,5,6,7,8,9].

Please optimize your algorithm to use less time and space. The input size may be as large as 5,000,000.

```
class Solution(object):
    def lexicalOrder(self, n):
        """
        :type n: int
        :rtype: List[int]
        """
        self.res = []
        def dfs(k, n):
            if k <= n:
                self.res.append(k)
                new = k*10
                if new <= n:
                    for i in range(0, 10):
                        dfs(new + i, n)
        for i in range(1,10):
            dfs(i, n)
        return self.res
```
