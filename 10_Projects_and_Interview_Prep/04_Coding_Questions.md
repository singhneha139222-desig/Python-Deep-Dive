# 📘 Coding Questions

## 📌 Introduction

This section contains common coding interview questions organized by difficulty. Each problem includes the approach, solution, and complexity analysis.

---

## 🟢 Easy Problems

### 1. Two Sum

**Problem:** Given an array of integers and a target, return indices of two numbers that add up to the target.

```python
def two_sum(nums: list, target: int) -> list:
    """
    Approach: Use hash map for O(1) lookup
    Time: O(n), Space: O(n)
    """
    seen = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    return []

# Test
print(two_sum([2, 7, 11, 15], 9))  # [0, 1]
print(two_sum([3, 2, 4], 6))       # [1, 2]
```

### 2. Reverse String

**Problem:** Reverse a string in-place.

```python
def reverse_string(s: list) -> None:
    """
    Approach: Two pointers from both ends
    Time: O(n), Space: O(1)
    """
    left, right = 0, len(s) - 1
    while left < right:
        s[left], s[right] = s[right], s[left]
        left += 1
        right -= 1

# Test
chars = ['h', 'e', 'l', 'l', 'o']
reverse_string(chars)
print(chars)  # ['o', 'l', 'l', 'e', 'h']
```

### 3. Valid Palindrome

**Problem:** Check if a string is a palindrome (considering only alphanumeric characters).

```python
def is_palindrome(s: str) -> bool:
    """
    Approach: Two pointers, skip non-alphanumeric
    Time: O(n), Space: O(1)
    """
    left, right = 0, len(s) - 1
    
    while left < right:
        while left < right and not s[left].isalnum():
            left += 1
        while left < right and not s[right].isalnum():
            right -= 1
        
        if s[left].lower() != s[right].lower():
            return False
        
        left += 1
        right -= 1
    
    return True

# Test
print(is_palindrome("A man, a plan, a canal: Panama"))  # True
print(is_palindrome("race a car"))  # False
```

### 4. Maximum Subarray (Kadane's Algorithm)

**Problem:** Find the contiguous subarray with the largest sum.

```python
def max_subarray(nums: list) -> int:
    """
    Approach: Kadane's Algorithm
    Time: O(n), Space: O(1)
    """
    max_sum = current_sum = nums[0]
    
    for num in nums[1:]:
        current_sum = max(num, current_sum + num)
        max_sum = max(max_sum, current_sum)
    
    return max_sum

# Test
print(max_subarray([-2, 1, -3, 4, -1, 2, 1, -5, 4]))  # 6 ([4,-1,2,1])
```

### 5. Valid Parentheses

**Problem:** Check if parentheses are balanced.

```python
def is_valid(s: str) -> bool:
    """
    Approach: Use stack to match pairs
    Time: O(n), Space: O(n)
    """
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}
    
    for char in s:
        if char in mapping:
            if not stack or stack[-1] != mapping[char]:
                return False
            stack.pop()
        else:
            stack.append(char)
    
    return len(stack) == 0

# Test
print(is_valid("()[]{}"))  # True
print(is_valid("([)]"))    # False
print(is_valid("{[]}"))    # True
```

---

## 🟡 Medium Problems

### 6. Longest Substring Without Repeating Characters

**Problem:** Find the length of the longest substring without repeating characters.

```python
def length_of_longest_substring(s: str) -> int:
    """
    Approach: Sliding window with hash set
    Time: O(n), Space: O(min(n, alphabet))
    """
    char_set = set()
    left = 0
    max_length = 0
    
    for right in range(len(s)):
        while s[right] in char_set:
            char_set.remove(s[left])
            left += 1
        
        char_set.add(s[right])
        max_length = max(max_length, right - left + 1)
    
    return max_length

# Test
print(length_of_longest_substring("abcabcbb"))  # 3 ("abc")
print(length_of_longest_substring("bbbbb"))     # 1 ("b")
```

### 7. 3Sum

**Problem:** Find all unique triplets that sum to zero.

```python
def three_sum(nums: list) -> list:
    """
    Approach: Sort + two pointers
    Time: O(n²), Space: O(1) excluding output
    """
    nums.sort()
    result = []
    
    for i in range(len(nums) - 2):
        # Skip duplicates
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        
        left, right = i + 1, len(nums) - 1
        
        while left < right:
            total = nums[i] + nums[left] + nums[right]
            
            if total < 0:
                left += 1
            elif total > 0:
                right -= 1
            else:
                result.append([nums[i], nums[left], nums[right]])
                
                # Skip duplicates
                while left < right and nums[left] == nums[left + 1]:
                    left += 1
                while left < right and nums[right] == nums[right - 1]:
                    right -= 1
                
                left += 1
                right -= 1
    
    return result

# Test
print(three_sum([-1, 0, 1, 2, -1, -4]))  # [[-1, -1, 2], [-1, 0, 1]]
```

### 8. Binary Search

**Problem:** Find target in sorted array.

```python
def binary_search(nums: list, target: int) -> int:
    """
    Approach: Divide and conquer
    Time: O(log n), Space: O(1)
    """
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1

# Test
print(binary_search([1, 2, 3, 4, 5, 6, 7], 5))  # 4
print(binary_search([1, 2, 3, 4, 5, 6, 7], 8))  # -1
```

### 9. Merge Intervals

**Problem:** Merge overlapping intervals.

```python
def merge_intervals(intervals: list) -> list:
    """
    Approach: Sort by start, merge if overlapping
    Time: O(n log n), Space: O(n)
    """
    if not intervals:
        return []
    
    # Sort by start time
    intervals.sort(key=lambda x: x[0])
    merged = [intervals[0]]
    
    for start, end in intervals[1:]:
        if start <= merged[-1][1]:  # Overlapping
            merged[-1][1] = max(merged[-1][1], end)
        else:
            merged.append([start, end])
    
    return merged

# Test
print(merge_intervals([[1,3], [2,6], [8,10], [15,18]]))
# [[1, 6], [8, 10], [15, 18]]
```

### 10. Group Anagrams

**Problem:** Group strings that are anagrams of each other.

```python
from collections import defaultdict

def group_anagrams(strs: list) -> list:
    """
    Approach: Use sorted string as key
    Time: O(n * k log k) where k is max string length
    Space: O(n * k)
    """
    groups = defaultdict(list)
    
    for s in strs:
        key = tuple(sorted(s))
        groups[key].append(s)
    
    return list(groups.values())

# Alternative: Use character count as key (faster)
def group_anagrams_v2(strs: list) -> list:
    """Time: O(n * k), Space: O(n * k)"""
    groups = defaultdict(list)
    
    for s in strs:
        count = [0] * 26
        for c in s:
            count[ord(c) - ord('a')] += 1
        groups[tuple(count)].append(s)
    
    return list(groups.values())

# Test
print(group_anagrams(["eat", "tea", "tan", "ate", "nat", "bat"]))
# [['eat', 'tea', 'ate'], ['tan', 'nat'], ['bat']]
```

---

## 🔴 Hard Problems

### 11. Merge K Sorted Lists

**Problem:** Merge k sorted linked lists into one sorted list.

```python
import heapq
from typing import List, Optional

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def merge_k_lists(lists: List[Optional[ListNode]]) -> Optional[ListNode]:
    """
    Approach: Min heap to always get smallest element
    Time: O(n log k) where n is total nodes, k is number of lists
    Space: O(k)
    """
    heap = []
    
    # Add first node from each list
    for i, node in enumerate(lists):
        if node:
            heapq.heappush(heap, (node.val, i, node))
    
    dummy = ListNode(0)
    current = dummy
    
    while heap:
        val, i, node = heapq.heappop(heap)
        current.next = node
        current = current.next
        
        if node.next:
            heapq.heappush(heap, (node.next.val, i, node.next))
    
    return dummy.next
```

### 12. Longest Palindromic Substring

**Problem:** Find the longest palindromic substring.

```python
def longest_palindrome(s: str) -> str:
    """
    Approach: Expand around center
    Time: O(n²), Space: O(1)
    """
    if not s:
        return ""
    
    start, end = 0, 0
    
    def expand_around_center(left: int, right: int) -> int:
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        return right - left - 1
    
    for i in range(len(s)):
        # Odd length palindrome
        len1 = expand_around_center(i, i)
        # Even length palindrome
        len2 = expand_around_center(i, i + 1)
        
        max_len = max(len1, len2)
        
        if max_len > end - start:
            start = i - (max_len - 1) // 2
            end = i + max_len // 2
    
    return s[start:end + 1]

# Test
print(longest_palindrome("babad"))  # "bab" or "aba"
print(longest_palindrome("cbbd"))   # "bb"
```

### 13. Word Break

**Problem:** Determine if string can be segmented into dictionary words.

```python
def word_break(s: str, word_dict: list) -> bool:
    """
    Approach: Dynamic programming
    Time: O(n² * k) where k is average word length
    Space: O(n)
    """
    word_set = set(word_dict)
    n = len(s)
    
    # dp[i] = True if s[0:i] can be segmented
    dp = [False] * (n + 1)
    dp[0] = True  # Empty string
    
    for i in range(1, n + 1):
        for j in range(i):
            if dp[j] and s[j:i] in word_set:
                dp[i] = True
                break
    
    return dp[n]

# Test
print(word_break("leetcode", ["leet", "code"]))  # True
print(word_break("applepenapple", ["apple", "pen"]))  # True
```

### 14. LRU Cache

**Problem:** Design a data structure for Least Recently Used (LRU) cache.

```python
from collections import OrderedDict

class LRUCache:
    """
    Approach: OrderedDict maintains insertion order
    Time: O(1) for both get and put
    Space: O(capacity)
    """
    
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = OrderedDict()
    
    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        # Move to end (most recently used)
        self.cache.move_to_end(key)
        return self.cache[key]
    
    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self.cache.move_to_end(key)
        self.cache[key] = value
        
        if len(self.cache) > self.capacity:
            # Remove least recently used (first item)
            self.cache.popitem(last=False)

# Test
cache = LRUCache(2)
cache.put(1, 1)
cache.put(2, 2)
print(cache.get(1))  # 1
cache.put(3, 3)      # Evicts key 2
print(cache.get(2))  # -1
```

### 15. Trapping Rain Water

**Problem:** Calculate how much water can be trapped after rain.

```python
def trap(height: list) -> int:
    """
    Approach: Two pointers
    Time: O(n), Space: O(1)
    """
    if not height:
        return 0
    
    left, right = 0, len(height) - 1
    left_max = right_max = 0
    water = 0
    
    while left < right:
        if height[left] < height[right]:
            if height[left] >= left_max:
                left_max = height[left]
            else:
                water += left_max - height[left]
            left += 1
        else:
            if height[right] >= right_max:
                right_max = height[right]
            else:
                water += right_max - height[right]
            right -= 1
    
    return water

# Test
print(trap([0,1,0,2,1,0,1,3,2,1,2,1]))  # 6
```

---

## 📊 Common Patterns Summary

| Pattern | Problems | Key Technique |
|---------|----------|---------------|
| Two Pointers | Two Sum, 3Sum, Container | Start/end or slow/fast |
| Sliding Window | Longest Substring | Expand/contract window |
| Hash Map | Anagrams, Two Sum | O(1) lookup |
| Binary Search | Sorted arrays | Divide and conquer |
| Dynamic Programming | Word Break, LCS | Subproblem memoization |
| Stack | Valid Parentheses | LIFO for matching |
| Heap | Merge K Lists | Priority queue |

---

## ⏭️ Next: MCQs

Test your knowledge with **[Multiple Choice Questions](05_MCQs.md)**!

---

*Practice makes perfect!* 💪
