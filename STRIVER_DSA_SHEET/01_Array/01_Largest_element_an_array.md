# 🌟 Largest Element in Array

<div align="center">

![Difficulty](https://img.shields.io/badge/Difficulty-🟢%20Easy-brightgreen?style=for-the-badge)
![Topics](https://img.shields.io/badge/Topics-Array%20|%20Linear%20Search-blue?style=for-the-badge)
![Time Complexity](https://img.shields.io/badge/Time-O\(n\)-orange?style=for-the-badge)
![Space Complexity](https://img.shields.io/badge/Space-O\(1\)-red?style=for-the-badge)

**🔗 [Solve on GFG](https://www.geeksforgeeks.org/problems/largest-element-in-array4009/1)**
**📖 [Striver Tutorial](https://takeuforward.org/data-structure/find-the-largest-element-in-an-array/)**

</div>

---

## 📋 Problem Statement

Given an array `arr[]` of size **N**, the task is to find the **largest element** present in the array.

### Example

```
Input: arr = [1, 8, 7, 56, 90]
Output: 90
```

```
Input: arr = [5, 5, 5, 5]
Output: 5
```

### Constraints

* `1 ≤ N ≤ 10^6`
* `0 ≤ arr[i] ≤ 10^6`

---

## 📖 Problem Analysis

* We need to find the **maximum value** in the array.
* Brute force: Compare every element with every other element → **O(n²)**.
* Optimal: Just traverse once and keep track of the **maximum so far** → **O(n)**.

---

## 🔄 Algorithm

```
ALGORITHM: FindLargestElement
INPUT: Array arr[] of size n
OUTPUT: Maximum element in array

BEGIN
    1. Initialize maxElement = arr[0]
    2. FOR i = 1 TO n-1 DO
          IF arr[i] > maxElement THEN
              maxElement = arr[i]
          END IF
       END FOR
    3. RETURN maxElement
END
```

---

## 🖼️ Visual Representation

### Example: arr = \[3, 7, 2, 9, 5]

Step-by-step process:

```
Start: max = 3
Compare arr[1]=7 > max → max=7
Compare arr[2]=2 < max → no change
Compare arr[3]=9 > max → max=9
Compare arr[4]=5 < max → no change

Final Answer = 9
```

Diagram:

```
Array: [3, 7, 2, 9, 5]
Max progression:
3 → 7 → 7 → 9 → 9
```

---

## ⚡ Complexity Analysis

* **Time Complexity:** O(n)

  * We traverse the array once.
* **Space Complexity:** O(1)

  * Only one variable `maxElement` used.

---

## 🚀 Approach Evolution (Brute → Optimal)

### 1. Brute Force Approach (O(n²))

Compare each element with every other element.

```cpp
class SolutionBrute {
public:
    int largest(vector<int>& arr) {
        for (int i = 0; i < arr.size(); i++) {
            bool isMax = true;
            for (int j = 0; j < arr.size(); j++) {
                if (arr[j] > arr[i]) {
                    isMax = false;
                    break;
                }
            }
            if (isMax) return arr[i];
        }
        return -1; // not possible
    }
};
```

---

### 2. Sorting Approach (O(n log n))

Sort array and take the last element.

```cpp
class SolutionSorting {
public:
    int largest(vector<int>& arr) {
        sort(arr.begin(), arr.end());
        return arr.back();
    }
};
```

---

### 3. Optimal Linear Scan (O(n)) ✅

```cpp
class Solution {
public:
    int largest(vector<int>& arr) {
        int maxNum = arr[0];
        for (int i = 1; i < arr.size(); i++) {
            if (arr[i] > maxNum) {
                maxNum = arr[i];
            }
        }
        return maxNum;
    }
};
```

---

## 💡 Intuition Behind the Solution

* To find the **largest element**, we just need to maintain a **running maximum**.
* At each step, compare the current element with the maximum so far.
* This works because the maximum of the whole array must be the maximum among all comparisons.

Think of it like:

```
You are scanning numbers one by one.  
Whenever you see a bigger number than your current "champion",  
you crown it as the new champion.
```

---

## 🔀 Comparison of Approaches

| Approach          | Time Complexity | Space Complexity | Notes                          |
| ----------------- | --------------- | ---------------- | ------------------------------ |
| Brute Force       | O(n²)           | O(1)             | Inefficient, checks every pair |
| Sorting           | O(n log n)      | O(1) or O(n)     | Wastes time sorting            |
| **Linear Scan** ✅ | **O(n)**        | **O(1)**         | Fastest & simplest             |

---

## 🎯 Edge Cases

1. Single element array: `[7] → 7`
2. All elements equal: `[5,5,5,5] → 5`
3. Increasing order: `[1,2,3,4] → 4`
4. Decreasing order: `[9,8,7,6] → 9`

---

✨ With this, the **Largest Element in Array** problem is solved optimally using a **single pass** approach.
