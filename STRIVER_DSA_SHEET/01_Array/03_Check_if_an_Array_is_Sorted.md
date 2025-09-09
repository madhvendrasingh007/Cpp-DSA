# Check If Array Is Sorted and Rotated

<div align="center">

![Difficulty](https://img.shields.io/badge/Difficulty-%F0%9F%92%A2%20Medium-yellow?style=for-the-badge)
![Topics](https://img.shields.io/badge/Topics-Array%20|%20Two%20Pointers-blue?style=for-the-badge)

[![LeetCode](https://img.shields.io/badge/Solve%20on-LeetCode-black?logo=leetcode\&style=for-the-badge)](https://leetcode.com/problems/check-if-array-is-sorted-and-rotated/description/)
[![Striver](https://img.shields.io/badge/%F0%9F%93%96%20Striver-Tutorial-orange?style=for-the-badge)](https://takeuforward.org/data-structure/check-if-an-array-is-sorted/)

</div>

---

## 1. Problem Statement & Constraints

Given an integer array `nums` of length **n**, return `true` if the array is **sorted in non-decreasing order and then rotated** (by one or more positions), otherwise return `false`.

* A **non-decreasingly sorted** array is one where every element is **greater than or equal** to the previous (i.e., `nums[i] ≥ nums[i−1]`).
* A **rotation** shifts some leading elements to the end, without changing their order.

Examples:

```
Input: nums = [3, 4, 5, 1, 2]  
Output: true  
Explanation: The original sorted array is [1, 2, 3, 4, 5] rotated by 3 positions.

Input: nums = [2, 1, 3, 4]  
Output: false  
Explanation: There are 2 violation points: 2→1 and 1→3, so cannot be a single rotation.
```

### Constraints:

* `n == nums.length`
* `1 ≤ n ≤ 10^5`
* `-10^6 ≤ nums[i] ≤ 10^6`

---

## 2. Optimal Approach (Striver-style, C++ — class only)

```cpp
class Solution {
public:
    bool check(vector<int>& nums) {
        int countDrop = 0, n = nums.size();
        for (int i = 1; i < n; ++i)
            if (nums[i] < nums[i - 1])
                ++countDrop;
        if (nums[0] < nums[n - 1])
            ++countDrop;
        return countDrop <= 1;
    }
};
```

---

## 3. Algorithm

1. Initialize a counter `countDrop = 0`.
2. Loop through `i = 1 to n - 1`:

   * If `nums[i] < nums[i - 1]`, increment `countDrop`.
3. After the loop, check if the **first element** is less than the **last element**; if yes, it indicates a wrap-around drop, so increment `countDrop` once more.
4. If `countDrop` is **0 or 1**, return `true`. More than one drop means it's **not** sorted-then-rotated, so return `false`.

---

## 4. Visual Diagram

Consider `nums = [3, 4, 5, 1, 2]`

```
Indices: 0   1   2   3   4    (0-based)
Values:  3 → 4 → 5 ↘ 1 → 2

Drops detected:
5 → 1 (i = 3) → countDrop = 1
Check wrap: 3 (first) < 2 (last) → true → countDrop = 2

Result: Actually, countDrop ≤ 1 must be true, but here...
Wait, logic wise: since nums[0] < nums[n - 1]? 3 < 2 false → no extra increment.

Final countDrop = 1 → valid rotation.
```

This visually helps track the two pointers and how the “drop” points are detected.

---

## 5. Complexity Analysis

| Metric           | Value                                |
| ---------------- | ------------------------------------ |
| Time Complexity  | O(n) — single pass through the array |
| Space Complexity | O(1) — constant auxiliary space      |

---

## 6. Other Approaches

### Brute Force (conceptual)

* Generate all possible rotations of the array.
* Check for each if it becomes sorted in non-decreasing order.
* Verdict is true if **any** rotation works.

```cpp
class BruteSolution {
public:
    bool check(vector<int>& nums) {
        int n = nums.size();
        for (int k = 0; k < n; ++k) {
            if (isSorted(nums)) return true;
            rotate(nums.begin(), nums.begin() + 1, nums.end());
        }
        return false;
    }
private:
    bool isSorted(const vector<int>& arr) {
        for (int i = 1; i < arr.size(); ++i)
            if (arr[i] < arr[i - 1])
                return false;
        return true;
    }
};
```

### Sorting-based check (semi-optimal)

* Clone the array to `orig`.
* Sort `orig`.
* Check if `nums` is a rotation of `orig` by string matching or index mapping.
* More overhead: O(n log n) time + O(n) space.

```cpp
class SortingSolution {
public:
    bool check(vector<int>& nums) {
        vector<int> orig = nums;
        sort(orig.begin(), orig.end());
        int n = nums.size();
        for (int shift = 0; shift < n; ++shift) {
            bool ok = true;
            for (int i = 0; i < n; ++i)
                if (nums[(shift + i) % n] != orig[i]) {
                    ok = false;
                    break;
                }
            if (ok) return true;
        }
        return false;
    }
};
```

---

## 7. Intuition Behind the Solution

To verify if the array is sorted **and then** rotated, track how many times the sequence **drops** (a decrease from one element to the next). In a perfectly sorted, non-decreasing array, there are **zero drops**. A rotation introduces exactly **one drop** when the smallest element jumps to the front.

If there are **more than one drop**, the sequence cannot be both sorted and rotated. A wrap-around drop (boundary condition) is the last check (`nums[0] < nums[n - 1]`).

---

## 8. Comparison of Approaches

| Approach                 | Time Complexity | Space Complexity | Notes                                         |
| ------------------------ | --------------- | ---------------- | --------------------------------------------- |
| Brute-force rotations    | O(n²)           | O(n)             | Unnecessary and inefficient for large `n`     |
| Sort + rotation check    | O(n log n)      | O(n)             | Cleaner logic but heavier due to sort step    |
| **Linear drop counting** | **O(n)**        | **O(1)**         | Elegant and optimal, handles wrap-around drop |

---

## Summary

This README dives deep with:

* A clear breakdown of the **problem** and its **constraints**.
* A visual and intuitive explanation for the **optimal approach**.
* An **efficient algorithm**, with both **complexity analysis** and **logical justification**.
* A comparison showing why the **linear drop-counting method** is superior.
