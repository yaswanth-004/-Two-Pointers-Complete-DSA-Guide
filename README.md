# 👉👈 Two Pointers — Complete DSA Guide

> Find pairs, triplets, detect cycles, and solve classic array problems in O(n)

---

## 📌 What is Two Pointers?

The **Two Pointer** technique uses two indices that move through an array (or linked list) to solve problems more efficiently than brute force.

```
Opposite ends:          [1, 2, 3, 4, 5, 6]
                         ↑               ↑
                        left           right

Same direction (slow/fast):  [1, 2, 3, 4, 5]
                               ↑  ↑
                              slow fast
```

---

## 🔧 Types

| Type | Pointers Start | Move Direction |
|------|---------------|----------------|
| Opposite Ends | left=0, right=n-1 | Towards each other |
| Same Direction | both at 0 | Right only |
| Fast & Slow | both at head/0 | Different speeds |

---

## 📚 Classic Problems

---

### 🔷 1. Two Sum (Sorted Array)

**Problem:** Find two numbers that sum to target.

```
Input:  arr = [1, 2, 3, 4, 6], target = 6
Output: [1, 3]  (indices, or values 2 and 4)
```

```python
def two_sum_sorted(arr, target):
    left, right = 0, len(arr) - 1

    while left < right:
        curr_sum = arr[left] + arr[right]

        if curr_sum == target:
            return [left, right]
        elif curr_sum < target:
            left += 1       # need bigger sum → move left right
        else:
            right -= 1      # need smaller sum → move right left

    return []

# Time: O(n)  Space: O(1)
```

---

### 🔷 2. Three Sum (Triplets that sum to zero)

**Problem:** Find all unique triplets that sum to 0.

```
Input:  [-1, 0, 1, 2, -1, -4]
Output: [[-1,-1,2], [-1,0,1]]
```

```python
def three_sum(arr):
    arr.sort()
    result = []

    for i in range(len(arr) - 2):
        if i > 0 and arr[i] == arr[i-1]:
            continue    # skip duplicates

        left, right = i + 1, len(arr) - 1

        while left < right:
            total = arr[i] + arr[left] + arr[right]

            if total == 0:
                result.append([arr[i], arr[left], arr[right]])
                while left < right and arr[left] == arr[left+1]:
                    left += 1
                while left < right and arr[right] == arr[right-1]:
                    right -= 1
                left += 1
                right -= 1
            elif total < 0:
                left += 1
            else:
                right -= 1

    return result

# Time: O(n²)  Space: O(1) (excluding output)
```

---

### 🔷 3. Container With Most Water

**Problem:** Find two lines forming a container with max water.

```
Input:  height = [1,8,6,2,5,4,8,3,7]
Output: 49
```

```python
def max_area(height):
    left, right = 0, len(height) - 1
    max_water = 0

    while left < right:
        h = min(height[left], height[right])
        w = right - left
        max_water = max(max_water, h * w)

        if height[left] < height[right]:
            left += 1     # move the shorter wall
        else:
            right -= 1

    return max_water

# Time: O(n)  Space: O(1)
```

---

### 🔷 4. Remove Duplicates from Sorted Array

```python
def remove_duplicates(arr):
    if not arr:
        return 0

    slow = 0    # points to last unique

    for fast in range(1, len(arr)):
        if arr[fast] != arr[slow]:
            slow += 1
            arr[slow] = arr[fast]

    return slow + 1

# Time: O(n)  Space: O(1)
```

---

### 🔷 5. Move Zeros to End

```python
def move_zeros(arr):
    slow = 0    # next position for non-zero

    for fast in range(len(arr)):
        if arr[fast] != 0:
            arr[slow], arr[fast] = arr[fast], arr[slow]
            slow += 1

# Time: O(n)  Space: O(1)
```

---

### 🔷 6. Sort Colors (Dutch National Flag)

**Problem:** Sort array of 0s, 1s, 2s in one pass.

```
Input:  [2, 0, 2, 1, 1, 0]
Output: [0, 0, 1, 1, 2, 2]
```

```python
def sort_colors(arr):
    low, mid, high = 0, 0, len(arr) - 1

    while mid <= high:
        if arr[mid] == 0:
            arr[low], arr[mid] = arr[mid], arr[low]
            low += 1
            mid += 1
        elif arr[mid] == 1:
            mid += 1
        else:
            arr[mid], arr[high] = arr[high], arr[mid]
            high -= 1

# Time: O(n)  Space: O(1)
```

---

### 🔷 7. Find the Duplicate (Pigeonhole / Floyd's Cycle)

**Problem:** Array of n+1 integers in range [1,n]. Find the duplicate. No extra space!

```
Input:  [1, 3, 4, 2, 2]
Output: 2
```

```python
def find_duplicate(arr):
    # Phase 1: Detect cycle (fast & slow pointers)
    slow = arr[0]
    fast = arr[0]

    while True:
        slow = arr[slow]           # move 1 step
        fast = arr[arr[fast]]      # move 2 steps
        if slow == fast:
            break

    # Phase 2: Find cycle entrance (= duplicate)
    slow = arr[0]
    while slow != fast:
        slow = arr[slow]
        fast = arr[fast]

    return slow

# Time: O(n)  Space: O(1)
# This is Floyd's Cycle Detection (Tortoise & Hare)
```

---

### 🔷 8. Detect Loop in Linked List (Floyd's Algorithm)

```python
class ListNode:
    def __init__(self, val):
        self.val = val
        self.next = None

def has_cycle(head):
    slow = head
    fast = head

    while fast and fast.next:
        slow = slow.next           # 1 step
        fast = fast.next.next      # 2 steps

        if slow == fast:
            return True            # cycle detected

    return False

# Time: O(n)  Space: O(1)
```

### Find Start of Loop

```python
def detect_cycle_start(head):
    slow = fast = head

    # Phase 1: detect
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            break
    else:
        return None   # no cycle

    # Phase 2: find start
    slow = head
    while slow != fast:
        slow = slow.next
        fast = fast.next

    return slow   # cycle start node
```

---

### 🔷 9. Palindrome Check (Two Pointers)

```python
def is_palindrome(s):
    left, right = 0, len(s) - 1

    while left < right:
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1

    return True

# Time: O(n)  Space: O(1)
```

---

### 🔷 10. Squares of Sorted Array

**Problem:** Given sorted array with negatives, return squares in sorted order.

```
Input:  [-4, -1, 0, 3, 10]
Output: [0, 1, 9, 16, 100]
```

```python
def sorted_squares(arr):
    n = len(arr)
    result = [0] * n
    left, right = 0, n - 1
    pos = n - 1

    while left <= right:
        l_sq = arr[left] ** 2
        r_sq = arr[right] ** 2

        if l_sq > r_sq:
            result[pos] = l_sq
            left += 1
        else:
            result[pos] = r_sq
            right -= 1

        pos -= 1

    return result

# Time: O(n)  Space: O(n)
```

---

### 🔷 11. Pigeonhole Principle — Find Missing Number

```python
def missing_number(arr):
    # n numbers in range [0,n], one is missing
    n = len(arr)
    expected_sum = n * (n + 1) // 2
    return expected_sum - sum(arr)

# Time: O(n)  Space: O(1)
```

---

## 🗺️ Pattern Map

| Problem | Pointer Setup | Key Move |
|---------|--------------|----------|
| Two sum (sorted) | Opposite ends | Move based on sum vs target |
| Three sum | Fix one, opposite ends | Skip duplicates |
| Container water | Opposite ends | Move shorter side |
| Remove duplicates | Slow + fast | Move fast always |
| Sort colors | Low/mid/high | Dutch flag |
| Find duplicate | Fast & slow | Floyd's cycle |
| Detect loop | Fast & slow | 2x speed |
| Palindrome | Opposite ends | Compare & move in |

---

## ⏱️ Complexity Summary

| Problem | Time | Space |
|---------|------|-------|
| Two Sum (sorted) | O(n) | O(1) |
| Three Sum | O(n²) | O(1) |
| Container Water | O(n) | O(1) |
| Remove Duplicates | O(n) | O(1) |
| Find Duplicate | O(n) | O(1) |
| Sort Colors | O(n) | O(1) |

---

## 💡 When to Use Two Pointers?

✅ Array is **sorted** (or can be sorted)
✅ Looking for **pairs / triplets** that satisfy a condition
✅ Need to detect **cycles**
✅ **In-place** operations needed
✅ Need to **shrink/expand** from both ends
