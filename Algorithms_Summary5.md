386.Lexicographical Numbers

Given an integer n, return 1 - n in lexicographical order.

For example, given 13, return: [1,10,11,12,13,2,3,4,5,6,7,8,9].

Please optimize your algorithm to use less time and space. The input size may be as large as 5,000,000.

**Thoughts: use dfs find every possible combinations from 1 to 10 to 100.... as long as it is less than n**

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
282.Expression Add Operators

Given a string that contains only digits 0-9 and a target value, return all possibilities to add binary operators (not unary) +, -, or * between the digits so they evaluate to the target value.

Example 1:

Input: num = "123", target = 6
Output: ["1+2+3", "1*2*3"] 

**Thoughts: Corner case， when it starts with empty expr in the beginning.**

```
class Solution(object):
    def addOperators(self, num, target):
        """
        :type num: str
        :type target: int
        :rtype: List[str]
        """
        res = []
        def dfs(rest, expr, cur, prev):
            if not rest and cur == target:
                res.append(expr)
            else:
                for i in range(1, len(rest) + 1):
                    if i == 1 or rest[0] != '0':
                        val = int(rest[:i])
                        nums = rest[:i]
                        dfs(rest[i:], expr + '+' + nums, cur + val, val)
                        dfs(rest[i:], expr + '-' + nums, cur - val, -val)
                        dfs(rest[i:], expr + '*' + nums, cur - prev + prev * val, prev * val)
        # Handles start with empty expr
        for i in range(1, len(num) + 1):
            if i == 1 or num[0] != '0':
                head = int(num[:i])
                dfs(num[i:], num[:i], head, head)

        return res

```

20. Contains Duplicate III

Given an array of integers, find out whether there are two distinct indices i and j in the array such that the absolute difference between nums[i] and nums[j] is at most t and the absolute difference between i and j is at most k.

Example 1:

Input: nums = [1,2,3,1], k = 3, t = 0
Output: true

**Thoughts: bucket, maintain a bucket of size t. there is only two options, where in the same bucket or the near by bucket**

```
class Solution(object):
    def containsNearbyAlmostDuplicate(self, nums, k, t):
        """
        :type nums: List[int]
        :type k: int
        :type t: int
        :rtype: bool
        """
        if t < 0: return False
        n = len(nums)
        d = {}
        w = t + 1
        for i in xrange(n):
            m = nums[i] / w
            if m in d:
                return True
            if m - 1 in d and abs(nums[i] - d[m - 1]) < w:
                return True
            if m + 1 in d and abs(nums[i] - d[m + 1]) < w:
                return True
            d[m] = nums[i]
            if i >= k: del d[nums[i - k] / w]
        return False
```

50.Pow(x, n)

Implement pow(x, n), which calculates x raised to the power n (xn).

```
class Solution(object):
    def myPow(self, x, n):
        """
        :type x: float
        :type n: int
        :rtype: float
        """
        if n == 0 or x == 1: return 1
        if n == 1: return x
        # handle corner case where n = -32, convert it to positve 31 and plus n
        if n < 0: return 1.0/(x*self.myPow(x, -(n+1)))
        res = 1
        while(n > 1):
            if n%2 != 0:
                res *= x
            x *= x
            n /= 2
        return res*x
```
