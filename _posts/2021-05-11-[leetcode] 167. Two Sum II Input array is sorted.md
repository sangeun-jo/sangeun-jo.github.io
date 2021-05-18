---
layout: post
title: Two Sum II Input array is sorted
categories: Algorithm
tags: 
 - leetcode
 - Binary Search
 - Array
 - Two pointers
---

[leetcode 167번 문제](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

문제 이해 
* 정렬된 리스트에서 target과 일치하는 두 아이템 찾기  

```
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        start = 0
        end = len(numbers) -1
        while start < end:
            current_sum = numbers[start] + numbers[end]
            if target == current_sum:
                return [start +1, end+1]
            elif current_sum < target:
                start += 1
            else:
                end -=1
        return [-1,-1]
```

