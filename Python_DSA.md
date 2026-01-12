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
