1. 2-Sum (Hashmap):
```python
class Solution(object):
    def twoSum(self, nums, target):
        mapp={}
        for idx, ele in enumerate(nums):
            complement=target-ele
            if(mapp.get(complement)!= None):
                return [idx, mapp.get(complement)]
            mapp.update({ele:idx})  
```              
2. 3-Sum (Sorting+two pointer):
```python
class Solution(object):
    def threeSum(self, nums):
        nums.sort()
        n=len(nums)
        ans=[]
        for idx in range(n-2):
            if (idx>0 and nums[idx]==nums[idx-1]):
                continue
            if(nums[idx]>0): 
                break    
            left = idx + 1
            right = n-1
            while (left < right):
                sum=nums[idx]+nums[left]+nums[right]
                if sum==0:
                    ans.append([nums[idx],nums[left],nums[right]])
                    while (left<right and nums[left]==nums[left+1]):
                        left+=1
                    while (left<right and nums[right]==nums[right-1]):
                        right-=1
                    left+=1
                    right-=1
                elif sum<0:
                    left+=1
                else:
                    right-=1
        return ans
```
3. 4-Sum II (Read Counter and Generator in python)(O(N2))
```python
from collections import Counter

class Solution:
    def fourSumCount(self, nums1: list[int], nums2: list[int], nums3: list[int], nums4: list[int]) -> int:
        sum_map = Counter(a + b for a in nums1 for b in nums2)
        
        count = 0
        for c in nums3:
            for d in nums4:
                target = -(c + d)
                if target in sum_map:
                    count += sum_map[target]
                    
        return count
```
4. Set Matrix Zeroes(check first, mark first, set matrix, set first)
```python
class Solution(object):
    def setZeroes(self, matrix):
        n=len(matrix)
        m=len(matrix[0])
        first_row= False
        first_col=False

        for i in range(n):
            if matrix[i][0]==0:first_row=True

        for j in range(m):
            if matrix[0][j]==0:first_col=True

        for i in range(1,n):
            for j in range(1,m):
                if matrix[i][j] ==0:
                    matrix[i][0]=0
                    matrix[0][j]=0

        for i in range(1,n):
            for j in range(1,m):
                if matrix[0][j] == 0 or matrix[i][0] ==0:
                    matrix[i][j]=0
        if first_row:
            for i in range(n):
                  matrix[i][0]=0
        if first_col:
            for j in range(m):
                  matrix[0][j]=0
```
5. Rotate Image (swap row<->col,reverse row)
```python
class Solution(object):
    def rotate(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: None Do not return anything, modify matrix in-place instead.
        """
        for i in range(len(matrix)):
            for j in range(i+1,len(matrix[0])):
                matrix[i][j],matrix[j][i]=matrix[j][i],matrix[i][j]
                
        for i in range(len(matrix)):
            matrix[i].reverse()        
```
6. Reshape the matrix (flatten and then put elements)
```python
class Solution(object):
    def matrixReshape(self, mat, r, c):
        n=len(mat)
        m=len(mat[0])
        if r*c!=n*m:
            return mat
        result=[[0] * c for _ in range(r)]    
        for k in range(0,n*m):
            result[k//c][k%c]=mat[k//m][k%m]
        return result
```
7. Next Permutation (find pivot, replace with next large number from right, reverse last section)
```python
class Solution(object):
    def nextPermutation(self, nums):
        n=len(nums)
        p=-1
        for i in range(n-2,-1,-1):
            if nums[i]<nums[i+1]:
                p=i
                break
        if p==-1:
            nums.sort()
            return   

        for i in range(n-1,-1,-1):
            if nums[i]>nums[p]:
                nums[i],nums[p]=nums[p],nums[i]
                break
        nums[p+1:n]=nums[p+1:n][::-1]                
```
8. Longest Consecutive Sequence (Can't Sort as sort is O(nlogn)) (use Set to remove duplicate, accept start if start-1 doesnt exist, increase till start+1 exist) (O(n))
```python
class Solution(object):
    def longestConsecutive(self, nums):
        lookup_set=set(nums)
        longest_streak=0
        for ele in lookup_set:
            if ele-1 not in lookup_set:
                curr_num=ele
                curr_streak=1
                while curr_num+1 in lookup_set:
                    curr_num+=1
                    curr_streak+=1
                longest_streak=max(curr_streak, longest_streak)
        return longest_streak            

```
9. Product Of Array except self (left element product array, right element product array):
```python
def productExceptSelf(nums):
    length = len(nums)
    answer = [1] * length
    
    prefix = 1
    for i in range(length):
        answer[i] = prefix
        prefix *= nums[i]
        
    suffix = 1
    for i in range(length - 1, -1, -1):
        answer[i] *= suffix
        suffix *= nums[i]
        
    return answer
```
10. Subarray Sum equals k ( Traverse array once and check if map contains current prefix sum - k to the left, update subarray count, add to map the current prefix and move on)
```python
class Solution(object):
    def subarraySum(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        n=len(nums)
        s=0
        prefix_map={0:1}
        ans=0
        for i in range(n):
            s+=nums[i]
            if s-k in prefix_map:
                ans+=prefix_map[s-k]
            prefix_map[s]= prefix_map.get(s,0)+1
               
        return ans
```
11. Merge Intervals ( sort intervals, compare end of merged array last element with start of sorted array current array, update merged)
```python
class Solution(object):
    def merge(self, intervals):
        """
        :type intervals: List[List[int]]
        :rtype: List[List[int]]
        """
        intervals.sort()
        merged=[intervals[0]]
        for ele in intervals:
            if ele[0]<= merged[-1][1]:
                merged[-1][1]=max(ele[1],merged[-1][1])
            else:
                merged.append(ele)

        return merged            


```
12. Insert Interval ( add first which end before start of newInterval, then add those which start before end of newInterval, then add rest which start after end of newInterval)
```python
class Solution(object):
    def insert(self, intervals, newInterval):
        result = []
        i = 0
        n = len(intervals)
        
        # Phase 1: Add all intervals ending before newInterval starts
        while i < n and intervals[i][1] < newInterval[0]:
            result.append(intervals[i])
            i += 1
            
        # Phase 2: Merge all overlapping intervals
        # While the current interval starts before the newInterval ends
        while i < n and intervals[i][0] <= newInterval[1]:
            newInterval[0] = min(newInterval[0], intervals[i][0])
            newInterval[1] = max(newInterval[1], intervals[i][1])
            i += 1
        
        # Add the final merged newInterval
        result.append(newInterval)
        
        # Phase 3: Add the remaining intervals
        while i < n:
            result.append(intervals[i])
            i += 1
            
        return result
```
