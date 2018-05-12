# Binary Search

33.Search in Rotated Sorted Array

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

**Thoughts: check mid point and determine whether target is on the left or right of mid**

> Note: >= is the key here 

```
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        if not nums:
            return -1
        start = 0 
        end = len(nums) - 1
        while start < end: 
            mid = (end + start) / 2
            if nums[mid] == target:
                return mid
            if nums[start] > target > nums[mid] or target > nums[mid] >= nums[start] or nums[mid] >= nums[start] > target:
                start = mid + 1
            else:
                end = mid
        return start if nums[start] == target else -1
```
34.Search for a Range

Given an array of integers sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].

For example,

Given [5, 7, 7, 8, 8, 10] and target value 8,

return [3, 4].

**Thoughts:Findfirst and FindLast**

> Note: Findfirst -> check start point first, FindLast -> check end point first

```
class Solution(object):
    def searchRange(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        if not nums:
            return [-1, -1]
        
        res = [-1, -1]
        if self.searchFirst(nums, target) == -1:
            return res
        else:
            res[0] = self.searchFirst(nums, target)
            res[1] = self.searchLast(nums, target)
            return res
    def searchFirst(self, nums, target):
        start = 0 
        end = len(nums) - 1
        while start + 1 < end:
            mid = start + (end - start)/2
            if nums[mid] >= target:
                end = mid
            else:
                start = mid
        if nums[start] == target:
            return start
        elif nums[end] == target:
            return end
        else:
            return -1
    def searchLast(self, nums, target):
        start = 0 
        end = len(nums) - 1
        while start + 1 < end:
            mid = start + (end - start)/2
            if nums[mid] <= target:
                start = mid
            else:
                end = mid
        if nums[end] == target:
            return end
        elif nums[start] == target:
            return start
        else:
            return -1
```
259.3Sum Smaller

Given an array of n integers nums and a target, find the number of index triplets i, j, k with 0 <= i < j < k < n that satisfy 

the condition nums[i] + nums[j] + nums[k] < target.

Example:

Input: nums = [-2,0,1,3], and target = 2

Output: 2 

Explanation: Because there are two triplets which sums are less than 2:
             [-2,0,1]
             [-2,0,3]
             
**Thoughts: Sort list first, use two pointers at start and at end, find three numbers less than target, 
if found, increase start else decrease end since it is sorted. counts increase end - start**

```
class Solution(object):
    def threeSumSmaller(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        nums[:] = sorted(nums)
        l = len(nums)
        count = 0
        for i in xrange(l):
            left = i + 1
            right = l - 1 
            while left < right:
                if nums[i] + nums[left] + nums[right] < target:
                    count += right - left
                    left += 1
                else:
                    right -= 1
                    
        return count
```



Game of life 



**Thoughts:


249.Group Shifted Strings

Given a string, we can "shift" each of its letter to its successive letter, for example: "abc" -> "bcd". We can keep 

"shifting" which forms the sequence:

"abc" -> "bcd" -> ... -> "xyz"

Given a list of strings which contains only lowercase alphabets, group all strings that belong to the same shifting sequence.

Example:

Input: ["abc", "bcd", "acef", "xyz", "az", "ba", "a", "z"],

Output: 
[
  ["abc","bcd","xyz"],
  ["az","ba"],
  ["acef"],
  ["a","z"]
]
**Thoughts:calcualte off set for each start word to 'a', and minus offset for all chars in word to get same key value for groupings.**

**Special case is when number is less than ord(a), plus 26 **

```
class Solution(object):
    def groupStrings(self, strings):
        """
        :type strings: List[str]
        :rtype: List[List[str]]
        """
        dict = collections.defaultdict(list)
        for s in strings:
            offset = ord(s[0]) - ord('a')
            key = ''
            for i in s:
                k = ord(i) - offset
                if k < ord('a'):
                    k = k + 26
                key += chr(k)
            dict[key].append(s)
        return [items for _, items in dict.items()]
```
406.Queue Reconstruction by Height

Suppose you have a random list of people standing in a queue. Each person is described by a pair of integers (h, k), where h is the height of the person and k is the number of people in front of this person who have a height greater than or equal to h. Write an algorithm to reconstruct the queue.

Note:
The number of people is less than 1,100.


Example

Input:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

Output:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]

**Thoughts: first sort by height and then sort by k, reversely, insert element by k to the final result**

```
class Solution(object):
    def reconstructQueue(self, people):
        """
        :type people: List[List[int]]
        :rtype: List[List[int]]
        """
        if not people:
            return []
        res = []
        people = sorted(people, key=lambda x:(x[0], -x[1]), reverse=True)
        for i in people:
            res.insert(i[1], i)
        return res
```

