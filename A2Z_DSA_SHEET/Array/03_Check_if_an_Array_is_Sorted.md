# 🔄 Check If Array Is Sorted and Rotated

<div align="center">

![Difficulty](https://img.shields.io/badge/Difficulty-🟢%20Easy-brightgreen?style=for-the-badge)
![Topics](https://img.shields.io/badge/Topics-Array%20|%20Two%20Pointers-blue?style=for-the-badge)
![Time Complexity](https://img.shields.io/badge/Time-O(n)-orange?style=for-the-badge)
![Space Complexity](https://img.shields.io/badge/Space-O(1)-red?style=for-the-badge)

**🔗 [Solve on LeetCode](https://leetcode.com/problems/check-if-array-is-sorted-and-rotated/description/)**

</div>

## 📋 Problem Statement

**Given an array `nums`, return `true` if the array was originally sorted in non-decreasing order, then rotated some number of positions (including zero). Otherwise, return `false`.**

**Note:** There may be duplicates in the original array.

### Examples:

```
Example 1:
Input: nums = [3,4,5,1,2]
Output: true
Explanation: [1,2,3,4,5] is the original sorted array.
You can rotate the array by x = 3 positions to begin on the element of value 3: [3,4,5,1,2].

Example 2:
Input: nums = [2,1,3,4]
Output: false
Explanation: There is no sorted array once rotated that can make nums.

Example 3:
Input: nums = [1,2,3]
Output: true
Explanation: [1,2,3] is already sorted and rotating by 0 positions gives [1,2,3].
```

### Constraints:
- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

### Topics Covered:
- 🔄 **Array Rotation**: Understanding circular array properties
- 📊 **Sorted Arrays**: Non-decreasing order validation
- 🔍 **Pattern Recognition**: Identifying rotation points
- 🧮 **Counting Logic**: Break point analysis

---

## 📖 Detailed Explanation

### Problem Analysis:
This problem requires us to determine if an array could have been obtained by rotating a sorted array. A **rotated sorted array** means we take a sorted array and move some elements from the front to the back (or vice versa).

### Key Insights:

1. **What is a Rotated Sorted Array?**
   - Original: `[1, 2, 3, 4, 5]`
   - Rotate by 2: `[4, 5, 1, 2, 3]`
   - The array maintains its sorted property in two parts

2. **Critical Observation**: 
   - In a rotated sorted array, there can be **at most ONE break point**
   - A break point is where `nums[i] > nums[i+1]`
   - If there are 0 break points → already sorted
   - If there is 1 break point → check if rotation is valid
   - If there are 2+ break points → not a rotated sorted array

3. **Validation Rules**:
   - Count break points where `nums[i] > nums[i+1]`
   - Also check the circular condition: `nums[n-1] > nums[0]`
   - Total "breaks" should be ≤ 1

### Step-by-Step Approach:

1. **Count Break Points**: Traverse array and count positions where order breaks
2. **Handle Circular Case**: Check if last element > first element
3. **Validate Count**: If total breaks ≤ 1, then it's valid

---

## 🔄 Algorithm

```
ALGORITHM: CheckSortedAndRotated
INPUT: Array nums[] of size n
OUTPUT: Boolean (true if sorted and rotated, false otherwise)

BEGIN
    1. Initialize breakCount = 0
    
    2. FOR i = 0 TO n-2 DO                    // Check adjacent pairs
           IF nums[i] > nums[i+1] THEN
               INCREMENT breakCount
           END IF
       END FOR
    
    3. IF nums[n-1] > nums[0] THEN            // Check circular condition
           INCREMENT breakCount
       END IF
    
    4. RETURN (breakCount <= 1)               // Valid if at most 1 break
END
```

### Pseudocode:
```
function checkSortedRotated(nums):
    breakCount = 0
    n = length(nums)
    
    // Count breaks in adjacent elements
    for i from 0 to n-2:
        if nums[i] > nums[i+1]:
            breakCount++
    
    // Check circular break (last to first)
    if nums[n-1] > nums[0]:
        breakCount++
    
    return breakCount <= 1
```

---

## 📊 Visual Diagram

### Case 1: Valid Rotated Array `[3,4,5,1,2]`
```
Original sorted: [1,2,3,4,5]
After rotation:  [3,4,5,1,2]

Array: [3, 4, 5, 1, 2]
Index:  0  1  2  3  4

Step-by-step analysis:
┌─────┬─────┬─────┬─────┬─────┐
│  3  │  4  │  5  │  1  │  2  │
└─────┴─────┴─────┴─────┴─────┘

Check adjacent pairs:
3 < 4 ✓ (no break)
4 < 5 ✓ (no break)  
5 > 1 ✗ (BREAK! count = 1)
1 < 2 ✓ (no break)

Check circular (last to first):
2 < 3 ✓ (no break)

Total breaks = 1 ≤ 1 → TRUE ✅
```

### Case 2: Invalid Array `[2,1,3,4]`
```
Array: [2, 1, 3, 4]
Index:  0  1  2  3

Step-by-step analysis:
┌─────┬─────┬─────┬─────┐
│  2  │  1  │  3  │  4  │
└─────┴─────┴─────┴─────┘

Check adjacent pairs:
2 > 1 ✗ (BREAK! count = 1)
1 < 3 ✓ (no break)
3 < 4 ✓ (no break)

Check circular (last to first):
4 > 2 ✗ (BREAK! count = 2)

Total breaks = 2 > 1 → FALSE ❌
```

### Case 3: Already Sorted `[1,2,3]`
```
Array: [1, 2, 3]
Index:  0  1  2

Check adjacent pairs:
1 < 2 ✓ (no break)
2 < 3 ✓ (no break)

Check circular:
3 > 1 ✗ (BREAK! count = 1)

Total breaks = 1 ≤ 1 → TRUE ✅
```

---

## ⚡ Complexity Analysis

### Time Complexity: **O(n)**
- **Single Pass**: We traverse the array exactly once
- **Constant Operations**: Each comparison takes O(1) time
- **Linear Scaling**: Processing time increases linearly with array size
- **Best/Average/Worst Case**: All O(n) since we must check every adjacent pair

### Space Complexity: **O(1)**
- **No Extra Data Structures**: Only using a counter variable
- **Constant Space**: Space usage doesn't depend on input size
- **In-place Analysis**: No modification of original array needed
- **Auxiliary Space**: Only `breakCount` variable = O(1)

### Detailed Analysis:
```
Operations Breakdown:
- Array traversal: n-1 comparisons
- Circular check: 1 comparison  
- Counter operations: O(1)
- Return statement: O(1)

Total: O(n) time, O(1) space
```

---

## 🚀 Approach Evolution: Brute Force to Optimal

### 1. Brute Force Approach (Inefficient)
```cpp
// Generate all possible rotations and check if any is sorted
class SolutionBruteForce {
public:
    bool check(vector<int>& nums) {
        int n = nums.size();
        
        // Try all possible rotations (0 to n-1)
        for (int rotations = 0; rotations < n; rotations++) {
            bool isSorted = true;
            
            // Check if current rotation is sorted
            for (int i = 0; i < n - 1; i++) {
                int curr = nums[(i + rotations) % n];
                int next = nums[(i + rotations + 1) % n];
                if (curr > next) {
                    isSorted = false;
                    break;
                }
            }
            
            if (isSorted) return true;
        }
        
        return false;
    }
};
```
- **Time Complexity**: O(n²) - n rotations × n comparisons each
- **Space Complexity**: O(1)
- **Why Inefficient?**: Redundant work checking every possible rotation

**Intuition**: "Try every possible starting point and see if the array becomes sorted"

### 2. Sorting + Rotation Check Approach (Memory Intensive)
```cpp
// Sort the array and check if original can be obtained by rotating sorted
class SolutionSorting {
public:
    bool check(vector<int>& nums) {
        vector<int> sorted = nums;
        sort(sorted.begin(), sorted.end());
        
        int n = nums.size();
        // Try all rotations of sorted array
        for (int i = 0; i < n; i++) {
            if (isRotation(nums, sorted, i)) {
                return true;
            }
        }
        return false;
    }
    
private:
    bool isRotation(vector<int>& original, vector<int>& sorted, int start) {
        int n = original.size();
        for (int i = 0; i < n; i++) {
            if (original[i] != sorted[(i + start) % n]) {
                return false;
            }
        }
        return true;
    }
};
```
- **Time Complexity**: O(n log n + n²) - sorting + rotation checks
- **Space Complexity**: O(n) - extra sorted array
- **Why Suboptimal?**: Unnecessary sorting and extra space

**Intuition**: "Sort the array first, then check if original is a rotation of sorted version"

### 3. Two-Pass Approach (Redundant)
```cpp
// First pass: find rotation point, Second pass: validate
class SolutionTwoPass {
public:
    bool check(vector<int>& nums) {
        int n = nums.size();
        int breakPoint = -1;
        int breakCount = 0;
        
        // First pass: find break points
        for (int i = 0; i < n - 1; i++) {
            if (nums[i] > nums[i + 1]) {
                breakCount++;
                breakPoint = i;
                if (breakCount > 1) return false;
            }
        }
        
        // Handle circular case
        if (nums[n-1] > nums[0]) {
            breakCount++;
        }
        
        return breakCount <= 1;
    }
};
```
- **Time Complexity**: O(n)
- **Space Complexity**: O(1)
- **Why Redundant?**: Can be done in single pass

**Intuition**: "Find where the array breaks, then validate if it's a valid rotation"

### 4. Optimal Single-Pass Solution ✅
```cpp
// Count break points in a single traversal
class Solution {
public:
    bool check(vector<int>& nums) {
        int breakCount = 0;
        int n = nums.size();
        
        // Check all adjacent pairs + circular condition
        for (int i = 0; i < n; i++) {
            // Compare current with next (handle circular with modulo)
            if (nums[i] > nums[(i + 1) % n]) {
                breakCount++;
                // Early termination: more than 1 break = invalid
                if (breakCount > 1) return false;
            }
        }
        
        return true; // breakCount will be 0 or 1
    }
};

// Alternative clean implementation
class SolutionAlternative {
public:
    bool check(vector<int>& nums) {
        int breaks = 0;
        int n = nums.size();
        
        // Count adjacent breaks
        for (int i = 0; i < n - 1; i++) {
            if (nums[i] > nums[i + 1]) breaks++;
        }
        
        // Count circular break
        if (nums[n - 1] > nums[0]) breaks++;
        
        return breaks <= 1;
    }
};
```
- **Time Complexity**: O(n) - single pass through array
- **Space Complexity**: O(1) - only counter variable
- **Why Optimal?**: Minimal operations, early termination, direct solution

**Intuition**: "In a rotated sorted array, there can be at most one place where the order breaks"

---

## 💡 Intuition Behind the Solution

### The Core Insight:
> "A sorted array that has been rotated will have at most ONE position where a larger element is followed by a smaller element."

### Mental Model - The Circular Clock:
Think of a rotated sorted array like a clock face:
```
Original: [1, 2, 3, 4, 5]
Rotated:  [3, 4, 5, 1, 2]

Like a clock: 3→4→5→1→2→3 (circular)
                  ↑
              Only one "jump down"
```

### Key Realizations:

1. **Break Point Analysis**:
   - **No breaks**: `[1,2,3,4]` - already sorted
   - **One break**: `[3,4,1,2]` - valid rotation (4→1 is the break)
   - **Two+ breaks**: `[2,1,3,4]` - impossible to achieve by rotation

2. **Circular Property**:
   - Must check if `last_element > first_element`
   - This represents the "wrap-around" break in rotation

3. **Mathematical Foundation**:
   - **Rotation Invariant**: A rotated sorted array maintains its local order
   - **Transitivity**: If we can get from any element to any other following the circular path in non-decreasing order, it's valid

### Why This Works:

**Theorem**: *An array is a rotated sorted array if and only if it has at most one "break point" considering circular arrangement.*

**Proof Sketch**:
- **Forward Direction**: If array is rotated sorted, then at most one break exists (at rotation point)
- **Backward Direction**: If at most one break exists, we can "unrotate" to get sorted array

### Visual Understanding:
```
Valid Rotation: [4,5,6,1,2,3]
Breaks: 6→1 (only one break)
Circular: 3→4 (no break)
Result: Valid ✅

Invalid Case: [1,3,2,4]  
Breaks: 3→2 (break 1), 4→1 (circular break)
Total: 2 breaks
Result: Invalid ❌
```

---

## 🎯 Edge Cases & Considerations

### Edge Cases to Handle:

1. **Single Element**: `[5]` → Output: `true` (trivially sorted)
2. **Already Sorted**: `[1,2,3,4]` → Output: `true` (zero rotations)
3. **Reverse Sorted**: `[4,3,2,1]` → Output: `false` (impossible by rotation)
4. **All Same Elements**: `[2,2,2,2]` → Output: `true` (always valid)
5. **Two Elements Sorted**: `[1,2]` → Output: `true`
6. **Two Elements Rotated**: `[2,1]` → Output: `true` (one break)
7. **Duplicates**: `[2,1,1,2]` → Output: `true` (handle duplicates properly)

### Advanced Considerations:

```cpp
// Handling edge cases in implementation
bool check(vector<int>& nums) {
    if (nums.size() <= 1) return true;  // Single element always valid
    
    int breaks = 0;
    int n = nums.size();
    
    for (int i = 0; i < n; i++) {
        if (nums[i] > nums[(i + 1) % n]) {
            breaks++;
            if (breaks > 1) return false;  // Early termination
        }
    }
    
    return true;
}
```

### Corner Cases in Real Interviews:

1. **Empty Array**: Usually not given based on constraints
2. **Integer Overflow**: Not relevant for this problem's constraints
3. **Very Large Arrays**: Algorithm scales linearly
4. **Duplicate Elements**: Algorithm naturally handles duplicates

---

## 🏆 Why This Solution is Optimal

### Optimality Arguments:

1. **Theoretical Lower Bound**: O(n)
   - Must examine each element at least once
   - Cannot determine rotation validity without seeing all elements

2. **Space Efficiency**: O(1)
   - No auxiliary data structures needed
   - Only constant extra variables

3. **Early Termination**: 
   - Can exit early when breaks > 1
   - Practical optimization for invalid cases

4. **Single Pass**: 
   - No redundant work
   - Each element examined exactly once

### Comparison with Alternatives:

| Approach | Time | Space | Pros | Cons |
|----------|------|-------|------|------|
| Brute Force | O(n²) | O(1) | Simple logic | Too slow |
| Sorting | O(n log n) | O(n) | Intuitive | Unnecessary work |
| Two Pass | O(n) | O(1) | Clear structure | Redundant |
| **Optimal** | **O(n)** | **O(1)** | **Efficient, Clean** | **None** |

### Real-World Applications:

- **Database Systems**: Checking circular buffer integrity
- **System Design**: Validating rotated logs or data streams  
- **Algorithms**: Foundation for more complex rotation problems
- **Data Structures**: Circular array validation

This problem demonstrates elegant problem-solving: converting a complex-sounding problem into a simple counting exercise with deep mathematical insight! 🎯