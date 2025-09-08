# 🔄 Check If Array Is Sorted and Rotated

<div align="center">

![Difficulty](https://img.shields.io/badge/Difficulty-🟢%20Easy-brightgreen?style=for-the-badge)
![Topics](https://img.shields.io/badge/Topics-Array%20|%20Two%20Pointers-blue?style=for-the-badge)

[![LeetCode](https://img.shields.io/badge/Solve%20on-LeetCode-black?logo=leetcode\&style=for-the-badge)](https://leetcode.com/problems/check-if-array-is-sorted-and-rotated/description/)
[![Striver](https://img.shields.io/badge/📖%20Striver-Tutorial-orange?style=for-the-badge)](https://takeuforward.org/data-structure/find-the-largest-element-in-an-array/practice)

</div>  

---

## 📋 Problem Statement

You are given an array `nums`. Return **true** if the array was originally sorted in **non-decreasing order** and then rotated **some number of times (possibly zero)**. Otherwise, return **false**.

👉 **Note:** The array may contain duplicates.

---

### 🔹 Examples

```
Input: nums = [3,4,5,1,2]  
Output: true  
Explanation: [1,2,3,4,5] rotated by 3 → [3,4,5,1,2]  

Input: nums = [2,1,3,4]  
Output: false  

Input: nums = [1,2,3]  
Output: true  
```

---

### 🔹 Constraints

* `1 <= nums.length <= 100`
* `1 <= nums[i] <= 100`

---

## 🏹 Optimal Approach (C++)

```cpp
class Solution {
public:
    bool check(vector<int>& nums) {
        int n = nums.size();
        int breaks = 0;
        
        for (int i = 0; i < n; i++) {
            if (nums[i] > nums[(i + 1) % n]) {
                breaks++;
                if (breaks > 1) return false;
            }
        }
        return true;
    }
};
```

---

## 🔄 Algorithm

1. Initialize `breaks = 0`
2. Traverse the array:

   * Compare each element with its **next** (circularly using `% n`)
   * If `nums[i] > nums[(i+1) % n]`, increment `breaks`
3. If `breaks > 1` → return `false`
4. Otherwise, return `true`

---

## 🏗️ Visual Diagram

### Example: `[3,4,5,1,2]`

```
Original: [1,2,3,4,5]
Rotation: [3,4,5,1,2]

Array: [3, 4, 5, 1, 2]
Index:  0  1  2  3  4

Check pairs:
3 < 4 → ok
4 < 5 → ok
5 > 1 → break (count=1)
1 < 2 → ok
2 < 3 → ok (circular)

Total breaks = 1 → ✅ Valid
```

---

## 📊 Complexity Analysis

* **Time Complexity** → O(n) (single traversal)
* **Space Complexity** → O(1) (only counter variable)

---

## 🪜 Approach Evolution

### 1️⃣ Brute Force (Try all rotations)

```cpp
class SolutionBrute {
public:
    bool check(vector<int>& nums) {
        int n = nums.size();
        for (int r = 0; r < n; r++) {
            bool sorted = true;
            for (int i = 0; i < n - 1; i++) {
                if (nums[(i + r) % n] > nums[(i + r + 1) % n]) {
                    sorted = false; break;
                }
            }
            if (sorted) return true;
        }
        return false;
    }
};
```

* **Time Complexity** → O(n²)
* **Space Complexity** → O(1)

---

### 2️⃣ Sorting + Rotation Check

```cpp
class SolutionSorting {
public:
    bool check(vector<int>& nums) {
        vector<int> sorted = nums;
        sort(sorted.begin(), sorted.end());
        int n = nums.size();
        for (int r = 0; r < n; r++) {
            bool match = true;
            for (int i = 0; i < n; i++) {
                if (nums[i] != sorted[(i + r) % n]) {
                    match = false; break;
                }
            }
            if (match) return true;
        }
        return false;
    }
};
```

* **Time Complexity** → O(n log n + n²)
* **Space Complexity** → O(n)

---

### 3️⃣ Two-Pass Approach

```cpp
class SolutionTwoPass {
public:
    bool check(vector<int>& nums) {
        int n = nums.size(), breaks = 0;
        for (int i = 0; i < n - 1; i++) {
            if (nums[i] > nums[i + 1]) breaks++;
        }
        if (nums[n - 1] > nums[0]) breaks++;
        return breaks <= 1;
    }
};
```

* **Time Complexity** → O(n)
* **Space Complexity** → O(1)

---

### 4️⃣ Optimal Single-Pass ✅

```cpp
class SolutionOptimal {
public:
    bool check(vector<int>& nums) {
        int breaks = 0, n = nums.size();
        for (int i = 0; i < n; i++) {
            if (nums[i] > nums[(i + 1) % n]) {
                breaks++;
                if (breaks > 1) return false;
            }
        }
        return true;
    }
};
```

* **Time Complexity** → O(n)
* **Space Complexity** → O(1)

---

## 💡 Intuition

* A sorted + rotated array can have **at most one break point** where `nums[i] > nums[i+1]`.
* If **0 breaks** → already sorted.
* If **1 break** → valid rotation.
* If **>1 breaks** → not possible.

👉 Think of the array as a **circular clock**: numbers increase until a single drop, then wrap around.

---

## ⚔️ Comparison of Approaches

| Approach             | Time        | Space | Pros               | Cons           |
| -------------------- | ----------- | ----- | ------------------ | -------------- |
| Brute Force          | O(n²)       | O(1)  | Simple idea        | Very slow      |
| Sorting + Check      | O(n²+nlogn) | O(n)  | Intuitive          | Memory heavy   |
| Two Pass             | O(n)        | O(1)  | Easy to reason     | Slightly extra |
| **Optimal (Chosen)** | **O(n)**    | O(1)  | Clean, efficient ✅ | None           |

---

## 🎯 Takeaway

This problem looks tricky, but the **key is recognizing the single-break property**. Once that’s clear, the solution becomes an elegant **O(n), O(1)** check.
