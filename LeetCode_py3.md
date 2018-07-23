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
It took 940ms to run, with time complexity of O(n2). A better way is to use hash table, which reduce the time complexity to O(n).  

    class Solution(object):
	  def twoSum(self, nums, target):
		    if len(nums) <= 1: 
			      return False 
		    buff_dict = {} 
		    for i in range(len(nums)): 
			      if nums[i] in buff_dict: 
				        return [buff_dict[nums[i]], i] 
			      else: 
                buff_dict[target - nums[i]] = i
The optimized way only took 32ms.
