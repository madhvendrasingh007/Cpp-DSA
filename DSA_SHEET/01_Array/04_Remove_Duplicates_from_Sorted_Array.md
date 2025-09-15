# Remove Duplicates from Sorted Array

<div align="center">

![Difficulty](https://img.shields.io/badge/Difficulty-🟢%20Easy-brightgreen?style=for-the-badge)
![Topics](https://img.shields.io/badge/Topics-Array%20|%20Two%20Pointers-blue?style=for-the-badge)

[![LeetCode](https://img.shields.io/badge/Solve%20on-LeetCode-black?logo=leetcode\&style=for-the-badge)](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/)
[![Striver](https://img.shields.io/badge/📖%20Striver-Tutorial-orange?style=for-the-badge)](https://takeuforward.org/data-structure/remove-duplicates-in-place-from-sorted-array/)

</div>

---

## 1. Problem Statement & Constraints

Given a **sorted** integer array `nums`, remove the duplicates **in-place** such that each unique element appears only once. The relative order of elements must remain the same. Return *k*, the number of unique elements.

* After removing duplicates, the first *k* elements of `nums` should be the unique values.
* It's not required to modify the elements beyond the first *k* positions.

### Example

```
Input: nums = [1, 1, 2]
Output: k = 2, nums = [1, 2, _]
```

```
Input: nums = [0, 0, 1, 1, 1, 2, 2, 3, 3, 4]
Output: k = 5, nums = [0, 1, 2, 3, 4, _, _, _, _, _]
```

### Constraints

* `1 ≤ nums.length ≤ 10^5`
* `-10^4 ≤ nums[i] ≤ 10^4`
* `nums` is sorted in **non-decreasing order**

---

## 2. Optimal Approach (Striver-style, C++ — class only)

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.empty()) return 0;
        int write = 1;
        for (int read = 1; read < nums.size(); ++read) {
            if (nums[read] != nums[read - 1]) {
                nums[write++] = nums[read];
            }
        }
        return write;
    }
};
```

---

## 3. Algorithm

1. If the array is empty, return 0.
2. Initialize `write = 1`. This points to where the next unique element will be placed.
3. Loop `read` from 1 to `n - 1`:

   * If `nums[read] != nums[read - 1]` (i.e., a new unique value), assign `nums[write] = nums[read]`, then `write++`.
4. Return `write`, which is the count of unique elements.

---

## 4. Visual Diagram

Consider the array: `[0, 0, 1, 1, 2, 3, 3]`

```
Index:    0 1 2 3 4 5 6
Nums:     0 0 1 1 2 3 3
Write→     ^
Read→      (moves along)
Flow:
read=1: nums[1] == nums[0] (skip)
read=2: nums[2] != nums[1], write=1 → nums[1]=1, write=2
read=3: nums[3] == nums[2] (skip)
read=4: nums[4] != nums[3], write=2 → nums[2]=2, write=3
read=5: nums[5] != nums[4], write=3 → nums[3]=3, write=4
read=6: nums[6] == nums[5] (skip)

Final k = 4, Modified array front: [0,1,2,3]
```

This illustration demonstrates how two pointers (`read`, `write`) traverse and compact unique values in-place.

---

## 5. Complexity Analysis

| Metric               | Value                                     |
| -------------------- | ----------------------------------------- |
| **Time Complexity**  | O(n) — Single pass over array             |
| **Space Complexity** | O(1) — In-place with constant extra space |

---

## 6. Other Approaches

### 6.1 Brute-Force with Auxiliary Space (Not In-Place)

```cpp
class BruteSolution {
public:
    int removeDuplicates(vector<int>& nums) {
        vector<int> unique;
        for (int num : nums) {
            if (unique.empty() || unique.back() != num) {
                unique.push_back(num);
            }
        }
        for (int i = 0; i < unique.size(); ++i) {
            nums[i] = unique[i];
        }
        return unique.size();
    }
};
```

* **Time:** O(n)
* **Space:** O(n) — uses extra vector, violates in-place requirement.

### 6.2 Using STL (Compact Approach, Still Not In-Place)

```cpp
class StdSolution {
public:
    int removeDuplicates(vector<int>& nums) {
        auto it = unique(nums.begin(), nums.end());
        return distance(nums.begin(), it);
    }
};
```

* **Time:** O(n)
* **Space:** O(1) in practice, but uses internal iterator mechanisms (less explicit control).

---

## 7. Intuition Behind the Solution

Since the array is sorted, duplicates appear consecutively. We can use two pointers:

* The **read** pointer scans elements in order.
* The **write** pointer ensures placement of the next unique value.

When we encounter a new value (`nums[read] != nums[read-1]`), we overwrite the `write` index with this new value, effectively crushing duplicates and maintaining only unique ones at the array’s start.

---

## 8. Comparison of Approaches

| Approach                      | Time Complexity | Space Complexity | Notes                                        |
| ----------------------------- | --------------- | ---------------- | -------------------------------------------- |
| Brute-force with extra vector | O(n)            | O(n)             | Simple, but not in-place                     |
| STL `unique()` + `distance()` | O(n)            | O(1)             | Elegant, but less explicit than manual logic |
| **Two-pointer (optimal)**     | **O(n)**        | **O(1)**         | In-place, clear, and ideal for interviews    |

---

## Summary

This README offers:

* A clear **problem definition** with examples and constraints.
* An **optimal, in-place two-pointer approach** in C++ similar to Striver’s.
* A visual diagram to help you **reason intuitively** about the process.
* **Complexity analysis** and comparisons with alternate methods.
