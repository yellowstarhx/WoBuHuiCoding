# Learning Coding: Solutions for LeetCode using python 3  
  
**不会做，就去学**  
>Just do it.

## 1st Round
Accept! Please!

### 1. Two Sum  
My solution:  

    class Solution:
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        for e in range(1,len(nums)):
            if target - nums[e-1] in nums[e:]:
                return [e-1,e+nums[e:].index(target-nums[e-1])]
