# 🔄 Check If Array Is Sorted and Rotated

<div align="center">

![Difficulty](https://img.shields.io/badge/Difficulty-🟢%20Easy-brightgreen?style=for-the-badge)
![Topics](https://img.shields.io/badge/Topics-Array%20|%20Two%20Pointers-blue?style=for-the-badge)

[![LeetCode](https://img.shields.io/badge/Solve%20on-LeetCode-black?logo=leetcode\&style=for-the-badge)](https://leetcode.com/problems/check-if-array-is-sorted-and-rotated/description/)
[![Striver](https://img.shields.io/badge/📖%20Striver-Tutorial-orange?style=for-the-badge)](https://takeuforward.org/data-structure/find-the-largest-element-in-an-array/)

</div>  

---

## 📋 Problem Statement

Given an array `nums`, return **true** if the array was originally sorted in **non-decreasing order**, then rotated **some number of positions (possibly zero)**. Otherwise, return **false**.

👉 **Note:** There may be duplicates in the array.

### Example

```
Input: nums = [3,4,5,1,2]  
Output: true  
Explanation: [1,2,3,4,5] → rotate by 3 → [3,4,5,1,2]
```

---

### Constraints

* `1 <= nums.length <= 100`
* `1 <= nums[i] <= 100`

---

## 🏹 Optimal Approach (C++ — Striver’s/LeetCode Style)

```cpp
class Solution {
public:
    bool check(vector<int>& nums) {
        int n = nums.size();
        int breakCount = 0;
        
        for (int i = 0; i < n; i++) {
            if (nums[i] > nums[(i + 1) % n]) {
                breakCount++;
                if (breakCount > 1) return false; // more than 1 break → invalid
            }
        }
        return true;
    }
};
```

✅ **Why optimal?**

* Single traversal (O(n))
* Constant memory usage (O(1))
* Early exit if invalid

---

## 🔄 Algorithm

1. Initialize `breakCount = 0`
2. Traverse the array:

   * Compare each element with its **next** (circularly using modulo)
   * If `nums[i] > nums[(i+1)%n]`, increment `breakCount`
3. If `breakCount > 1` → return `false`
4. Else return `true`

---

## 🏗️ Visual Diagram (Architecture Flow)

```
        ┌──────────────────────────┐
        │ Input Array: nums[]       │
        └─────────────┬────────────┘
                      │
                      ▼
         ┌───────────────────────────┐
         │ Traverse all elements      │
         │ Compare nums[i] & nums[i+1]│
         └─────────────┬─────────────┘
                       │
        ┌──────────────┼──────────────┐
        │                              │
        ▼                              ▼
  If nums[i] > nums[i+1]          Else continue
      breakCount++                
        │
        ▼
 ┌───────────────┐
 │ breakCount > 1│───YES──► ❌ Return False
 └───────┬───────┘
         │NO
         ▼
     ✅ Return True
```

---

## 📊 Complexity Analysis

* **Time Complexity** → O(n) (single pass through array)
* **Space Complexity** → O(1) (only counter variable)

---

Perfect 👍 I’ll enhance the **Approach Evolution** section so that each approach has:

* A **small C++ class snippet** (only the key logic part, not full boilerplate)
* Its **time and space complexity**

Here’s the updated section for your README:

---

## 🪜 Approach Evolution (Brute → Optimal)

### 1️⃣ Brute Force Approach

Try all possible rotations and check if any is sorted.

```cpp
class SolutionBrute {
public:
    bool check(vector<int>& nums) {
        int n = nums.size();
        for (int r = 0; r < n; r++) {
            bool sorted = true;
            for (int i = 0; i < n - 1; i++) {
                if (nums[(i + r) % n] > nums[(i + r + 1) % n]) {
                    sorted = false;
                    break;
                }
            }
            if (sorted) return true;
        }
        return false;
    }
};
```

* **Time Complexity** → O(n²) (n rotations × n comparisons)
* **Space Complexity** → O(1)

---

### 2️⃣ Sorting + Rotation Check

Sort a copy of the array and check if original is a rotation of sorted.

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

* **Time Complexity** → O(n log n + n²) (sort + rotation checks)
* **Space Complexity** → O(n) (extra sorted array)

---

### 3️⃣ Two-Pass Approach

Find break point(s) first, then validate separately.

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

Count breaks directly in a single traversal including circular case.

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

* **Time Complexity** → O(n) (single traversal, early exit possible)
* **Space Complexity** → O(1)


---


## 💡 Intuition Behind the Solution

👉 A rotated sorted array has at most **one break point** (a spot where the order decreases).

* If **0 breaks** → already sorted.
* If **1 break** → valid rotation.
* If **>1 breaks** → impossible.

📌 Think of it like a **clock**:

```
[1,2,3,4,5] rotated → [3,4,5,1,2]  
Only one "jump down" (5 → 1)
```

---

## ⚔️ Comparison of Alternatives

| Approach        | Time     | Space | Pros                          | Cons               |
| --------------- | -------- | ----- | ----------------------------- | ------------------ |
| Brute Force     | O(n²)    | O(1)  | Simple idea                   | Very slow          |
| Sorting + Check | O(n²)    | O(n)  | Intuitive                     | Memory heavy       |
| Two Pass        | O(n)     | O(1)  | Easy to explain               | Slightly redundant |
| **Optimal**     | **O(n)** | O(1)  | Fastest & clean, early exit ✅ | None               |

---

## 🎯 Final Takeaway

This problem teaches us that **rotation doesn’t break sorting fully** — it only introduces **one possible disorder point**. Recognizing this pattern allows us to solve the problem in a clean **single pass with O(1) space**.
