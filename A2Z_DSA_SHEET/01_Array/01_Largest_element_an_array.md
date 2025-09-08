# 🔍 Find the Largest Element in an Array


# 🔄 Check If Array Is Sorted and Rotated

<div align="center">

![Difficulty](https://img.shields.io/badge/Difficulty-🟢%20Easy-brightgreen?style=for-the-badge)
![Topics](https://img.shields.io/badge/Topics-Array%20|%20Two%20Pointers-blue?style=for-the-badge)

**🔗 [Solve on LeetCode](https://leetcode.com/problems/check-if-array-is-sorted-and-rotated/description/)**

**📖 [Striver Tutorial](https://takeuforward.org/data-structure/find-the-largest-element-in-an-array/practice)**

</div>



## 📋 Problem Statement

**Given an array arr[], the task is to find the largest element and return it.**

### Examples:

```
Example 1:
Input: arr[] = [1, 8, 7, 56, 90]
Output: 90
Explanation: The largest element of the given array is 90.

Example 2:
Input: arr[] = [5, 5, 5, 5]
Output: 5
Explanation: The largest element of the given array is 5.

Example 3:
Input: arr[] = [10]
Output: 10
Explanation: There is only one element which is the largest.
```

### Constraints:
- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

***

## 🔄 Algorithm

```
ALGORITHM: FindLargestElement
INPUT: Array arr[] of size n
OUTPUT: Largest element in the array

BEGIN
    1. IF n == 0 THEN
           RETURN -1 or handle empty array
       END IF
    
    2. Initialize max = arr[0]                   // Assume first element is largest
    
    3. FOR i = 1 TO n-1 DO                      // Start from second element
           IF arr[i] > max THEN                 // Found larger element
               max = arr[i]                     // Update maximum
           END IF
       END FOR
    
    4. RETURN max                               // Return the largest element
END
```

### Pseudocode:
```
function findLargest(arr):
    if arr.length == 0:
        return null
    
    max = arr[0]
    
    for i from 1 to arr.length-1:
        if arr[i] > max:
            max = arr[i]
    
    return max
```

***

## 📊 Visual Diagram

### Case 1: Finding Maximum in `[2, 5, 1, 3, 0]`
```
Array: [2, 5, 1, 3, 0]
Index:  0  1  2  3  4

Step-by-step traversal:
┌─────┬─────┬─────┬─────┬─────┐
│  2  │  5  │  1  │  3  │  0  │
└─────┴─────┴─────┴─────┴─────┘

Initial: max = 2 (arr[0])

i=1: arr[1] = 5
     5 > 2? YES ✓
     max = 5

i=2: arr[2] = 1  
     1 > 5? NO ✗
     max = 5 (unchanged)

i=3: arr[3] = 3
     3 > 5? NO ✗  
     max = 5 (unchanged)

i=4: arr[4] = 0
     0 > 5? NO ✗
     max = 5 (unchanged)

Final Result: max = 5 ✅
```

### Case 2: All Elements Same `[7, 7, 7]`
```
Array: [7, 7, 7]
Index:  0  1  2

┌─────┬─────┬─────┐
│  7  │  7  │  7  │
└─────┴─────┴─────┘

Initial: max = 7

i=1: 7 > 7? NO ✗ → max = 7
i=2: 7 > 7? NO ✗ → max = 7

Result: max = 7 ✅
```

### Case 3: Descending Order `[9, 7, 5, 3, 1]`
```
Array: [9, 7, 5, 3, 1]
Index:  0  1  2  3  4

┌─────┬─────┬─────┬─────┬─────┐
│  9  │  7  │  5  │  3  │  1  │
└─────┴─────┴─────┴─────┴─────┘

Initial: max = 9

All comparisons:
7 > 9? NO ✗
5 > 9? NO ✗  
3 > 9? NO ✗
1 > 9? NO ✗

Result: max = 9 (first element) ✅
```

***

## ⚡ Complexity Analysis

### Time Complexity: **O(n)**
- **Single Pass**: We traverse the array exactly once
- **Constant Operations**: Each comparison takes O(1) time
- **Linear Growth**: Time increases proportionally with array size
- **Best/Average/Worst Case**: All O(n) - must check every element

### Space Complexity: **O(1)**
- **Constant Space**: Only one variable (`max`) needed
- **No Extra Arrays**: No additional data structures required
- **In-place Operation**: Original array remains unchanged
- **Auxiliary Space**: Just the `max` variable = O(1)

### Detailed Analysis:
```
Operations Breakdown:
- Array access: n times  
- Comparisons: n-1 times
- Assignments: At most n times (worst case: ascending order)
- Return: 1 time

Total: O(n) time, O(1) space
```

***

## 🚀 Approach Evolution: Brute Force to Optimal

### 1. Sorting Approach (Inefficient)
```cpp
class SolutionSorting {
public:
    int largest(vector &arr) {
        sort(arr.begin(), arr.end());
        return arr[arr.size() - 1];
    }
};
```
- **Time Complexity**: O(n log n) - due to sorting
- **Space Complexity**: O(1) or O(n) depending on sort implementation
- **Why Inefficient?**: Unnecessary sorting when we only need maximum

**Intuition**: "Sort everything and pick the last element"

### 2. Using STL max_element (Library Approach)
```cpp
class SolutionSTL {
public:
    int largest(vector &arr) {
        return *max_element(arr.begin(), arr.end());
    }
};
```
- **Time Complexity**: O(n) - internally does linear search
- **Space Complexity**: O(1)
- **Why Suboptimal?**: Depends on library, less control over implementation

**Intuition**: "Use built-in function to find maximum"

### 3. Recursive Approach (Functional Style)
```cpp
class SolutionRecursive {
public:
    int findMax(vector &arr, int n) {
        // Base case
        if (n == 1)
            return arr[0];
        
        // Recursive case
        return max(arr[n-1], findMax(arr, n-1));
    }
    
    int largest(vector &arr) {
        return findMax(arr, arr.size());
    }
};
```
- **Time Complexity**: O(n) - each element processed once
- **Space Complexity**: O(n) - due to recursion stack
- **Why Suboptimal?**: Extra space for recursion stack

**Intuition**: "Break problem into smaller subproblems recursively"

### 4. Optimal Iterative Solution ✅
```cpp
class Solution {
public:
    int largest(vector &arr) {
        int max_elem = arr[0];
        
        for (int i = 1; i  max_elem) {
                max_elem = arr[i];
            }
        }
        
        return max_elem;
    }
};
```
- **Time Complexity**: O(n) - single pass through array
- **Space Complexity**: O(1) - only one extra variable
- **Why Optimal?**: Minimal operations, no extra space, direct solution

**Intuition**: "Keep track of the best candidate seen so far"

***

## 💡 Intuition Behind the Solution

### The Core Insight:
> "To find the largest element, we only need to remember the biggest number we've seen so far and compare it with each new number we encounter."

### Mental Model - The Competition:
Think of this like a competition where each array element is a contestant:
```
Contestants: [2, 8, 3, 7, 1]

Round 1: 2 wins (by default, first contestant)
Round 2: 8 vs 2 → 8 wins! (new champion)
Round 3: 3 vs 8 → 8 wins! (champion remains)
Round 4: 7 vs 8 → 8 wins! (champion remains)  
Round 5: 1 vs 8 → 8 wins! (final champion)

Result: 8 is the ultimate winner!
```

### Key Realizations:

1. **Single Champion Principle**:
   - Only one "current maximum" needs to be maintained
   - Every new element either beats the champion or doesn't

2. **Linear Sufficiency**:
   - We must see every element at least once
   - No need to compare elements with each other, only with current max

3. **Greedy Strategy**:
   - Always keep the best candidate seen so far
   - Make locally optimal choices (update when finding larger element)

### Why This Works:

**Theorem**: *Any element that is the true maximum of an array will eventually become the "current maximum" during a left-to-right traversal and will never be replaced.*

**Proof Sketch**:
- **Correctness**: True maximum is larger than all other elements
- **Optimality**: Once we encounter the true maximum, no subsequent element can replace it
- **Completeness**: Linear scan ensures we encounter every element

### Visual Understanding:
```
Array: [3, 7, 2, 9, 1]
Max tracking: 
   3 → 7 → 7 → 9 → 9
   ↑   ↑   ↑   ↑   ↑
  max  new  same new same
 init  max   max  max  max

Final: 9 (correct maximum)
```

***

## 🏆 Comparison Between Alternatives

### Comprehensive Comparison:

| Approach | Time | Space | Pros | Cons | Use Case |
|----------|------|-------|------|------|----------|
| **Iterative** | **O(n)** | **O(1)** | **Simple, Optimal, Clear** | **None** | **Always preferred** |
| Sorting | O(n log n) | O(1)-O(n) | Easy to understand | Slower, overkill | When array needs sorting anyway |
| STL max_element | O(n) | O(1) | One-liner, readable | Library dependency | Quick prototyping |
| Recursive | O(n) | O(n) | Functional style | Stack overhead | Academic/functional programming |
| Divide & Conquer | O(n) | O(log n) | Parallelizable | Complex implementation | Parallel processing scenarios |

### When to Use Each Approach:

**Iterative Solution** ✅:
- **Production code**: Most reliable and efficient
- **Interviews**: Demonstrates algorithmic thinking
- **Large datasets**: Minimal memory overhead
- **General purpose**: Works in all scenarios

**Sorting Approach**:
- When you need both maximum and sorted array
- Educational purposes to show inefficiency
- One-time operations on small datasets

**STL Approach**:
- Rapid prototyping
- Code readability is priority over control
- Trust in standard library implementation

**Recursive Approach**:
- Functional programming paradigms
- Educational purposes (understanding recursion)
- When stack space is abundant

This problem beautifully demonstrates that the simplest approach is often the most elegant and efficient! The iterative solution combines optimal time complexity with minimal space usage and maximum clarity. 🎯