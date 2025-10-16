# 🔺 Pascal's Triangle

<div align="center">

[![Difficulty](https://img.shields.io/badge/Difficulty-🟢%20Easy-brightgreen?style=for-the-badge)](https://leetcode.com/problems/pascals-triangle/)
[![Topics](https://img.shields.io/badge/Topics-Array%20|%20Dynamic%20Programming-blue?style=for-the-badge)](https://leetcode.com/problems/pascals-triangle/)
[![Practice](https://img.shields.io/badge/Practice-LeetCode-orange?style=for-the-badge)](https://leetcode.com/problems/pascals-triangle/)
[![Article](https://img.shields.io/badge/Article-Medium-black?style=for-the-badge)](https://medium.com/@_monitsharma/daily-leetcode-problems-problem-118-pascals-triangle-e28daf50a153)

</div>

---

## 📋 Problem Statement

Given an integer `numRows`, return the first `numRows` of **Pascal's triangle**.

In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:

```
        1
       1 1
      1 2 1
     1 3 3 1
    1 4 6 4 1
```

### Examples:

**Example 1:**
```
Input: numRows = 5
Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```

**Example 2:**
```
Input: numRows = 1
Output: [[1]]
```

**Example 3:**
```
Input: numRows = 3
Output: [[1],[1,1],[1,2,1]]
```

### Constraints:
- `1 ≤ numRows ≤ 30`

---

## 🚀 Approach Comparison

| Approach | Time Complexity | Space Complexity | Difficulty | Best For |
|----------|-----------------|-----------------|-----------|----------|
| **Optimal Iterative (DP)** | O(n²) | O(n²) | ⭐ Easy | **Production & Interviews** |
| **Binomial Coefficient** | O(n² × k) | O(n²) | ⭐ Medium | Learning Mathematics |
| **Recursive** | O(2ⁿ) / O(n²) with memo | O(n²) | ⭐⭐ Medium | Understanding Recurrence |

---

## 💻 Solution 1: Optimal Iterative Approach (RECOMMENDED)

**Complexity: O(n²) Time | O(n²) Space**

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        // Initialize result vector to store all rows
        vector<vector<int>> triangle;
        
        // Generate each row of Pascal's Triangle
        for (int i = 0; i < numRows; i++) {
            // Create a new row with (i+1) elements, all initialized to 1
            vector<int> row(i + 1, 1);
            
            // Calculate inner elements (not first or last)
            // Each element is sum of two elements from previous row
            for (int j = 1; j < i; j++) {
                row[j] = triangle[i - 1][j - 1] + triangle[i - 1][j];
            }
            
            // Add completed row to triangle
            triangle.push_back(row);
        }
        
        return triangle;
    }
};
```

### 🔍 Detailed Explanation

This algorithm uses **dynamic programming** to build Pascal's Triangle row by row.

**Core Insight**: Each row in Pascal's Triangle can be generated from the previous row:
- **First and last elements** of each row are always `1`
- **Inner elements** are the sum of two adjacent elements from the previous row
- **Formula**: `triangle[i][j] = triangle[i-1][j-1] + triangle[i-1][j]`

**Algorithm Logic**:
1. Initialize an empty result vector
2. For each row from 0 to numRows-1:
   - Create a new row with (i+1) elements, all set to 1
   - Calculate middle elements using previous row
   - Add completed row to result
3. Return the complete triangle

**Example Trace (numRows = 5)**:
- **Row 0**: `[1]` (Base case)
- **Row 1**: `[1, 1]` (Both edges are 1)
- **Row 2**: `[1, 2, 1]` → Middle: 1 + 1 = 2
- **Row 3**: `[1, 3, 3, 1]` → Position 1: 1 + 2 = 3, Position 2: 2 + 1 = 3
- **Row 4**: `[1, 4, 6, 4, 1]` → Position 1: 1 + 3 = 4, Position 2: 3 + 3 = 6, Position 3: 3 + 1 = 4

### 📊 Algorithm Pseudocode

```
ALGORITHM GeneratePascalTriangle(numRows)
BEGIN
    triangle ← empty 2D array
    
    FOR i ← 0 TO numRows - 1 DO
        row ← array of size (i+1) filled with 1
        
        FOR j ← 1 TO i - 1 DO
            row[j] ← triangle[i-1][j-1] + triangle[i-1][j]
        END FOR
        
        triangle.append(row)
    END FOR
    
    RETURN triangle
END
```

### 🎯 Visual Diagram

```
Pascal's Triangle Generation Process:

Row 0:                    [1]
                           ↓
Row 1:                  [1, 1]
                         ↙   ↘
Row 2:              [1,  2,  1]
                      ↙  ↓ ↓  ↘
Row 3:          [1,  3,  3,  1]
                  ↙  ↓ ↓ ↓ ↓  ↘
Row 4:      [1,  4,  6,  4,  1]

Building Row 3 from Row 2:
Previous: [1, 2, 1]
           ↓  ↓  ↓
          1+0 1+2 2+1 1+0
           ↓   ↓   ↓   ↓
Current:  [1,  3,  3,  1]

Detailed Construction of Row 4:
Previous Row: [1, 3, 3, 1]
Step 1: Position 0 → 1 (edge)
Step 2: Position 1 → 1 + 3 = 4
Step 3: Position 2 → 3 + 3 = 6
Step 4: Position 3 → 3 + 1 = 4
Step 5: Position 4 → 1 (edge)

Result:       [1, 4, 6, 4, 1]

[Visual Pattern Recognition]
      1                Row 0: 1 element
     1 1               Row 1: 2 elements
    1 2 1              Row 2: 3 elements
   1 3 3 1             Row 3: 4 elements
  1 4 6 4 1            Row 4: 5 elements
 1 5 10 10 5 1         Row 5: 6 elements

Pattern: Row n has (n+1) elements
         Edges are always 1
         Inner values = sum of two above
```

### ⚡ Complexity Analysis

**Time Complexity: O(n²)**
- Outer loop: n iterations (for each row)
- Inner loop: For row i, calculate i-1 inner elements
- Total operations: 0 + 1 + 2 + ... + (n-1) = n(n-1)/2 ≈ O(n²)
- We compute and store every element in the triangle

**Space Complexity: O(n²)**
- Output space: Triangle contains 1 + 2 + 3 + ... + n = n(n+1)/2 elements
- Auxiliary space: O(1) (only loop variables)
- Total: O(n²) where n = numRows

Note: If excluding output space, auxiliary space is O(1).

### 💡 Why This Approach is Optimal

✅ **Mathematically Correct**: Based on Pascal's Triangle fundamental property
✅ **No Redundant Computation**: Each element calculated exactly once
✅ **Clean Code**: Simple, readable implementation
✅ **Efficient**: Achieves theoretical minimum time/space complexity
✅ **Cache Friendly**: Sequential memory access patterns
✅ **Scalable**: Handles maximum constraint (30 rows) efficiently

---

## 💻 Solution 2: Binomial Coefficient Approach

**Complexity: O(n² × k) Time | O(n²) Space**

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> triangle;
        
        for (int i = 0; i < numRows; i++) {
            vector<int> row;
            for (int j = 0; j <= i; j++) {
                // Calculate C(i, j) = i! / (j! × (i-j)!)
                row.push_back(binomialCoefficient(i, j));
            }
            triangle.push_back(row);
        }
        
        return triangle;
    }
    
private:
    int binomialCoefficient(int n, int k) {
        if (k > n - k) k = n - k;  // Optimization
        
        int result = 1;
        for (int i = 0; i < k; i++) {
            result *= (n - i);
            result /= (i + 1);
        }
        return result;
    }
};
```

### 🔍 Explanation

This approach uses the mathematical property that each element in Pascal's Triangle is a binomial coefficient C(n, k).

**Key Formula**: Each element at position (i, j) = C(i, j) = i! / (j! × (i-j)!)

**Algorithm**:
1. For each row i from 0 to numRows-1
2. For each position j from 0 to i
3. Calculate the binomial coefficient C(i, j)
4. Add it to the current row

**Advantages**:
- Direct mathematical calculation
- Independent row generation (can generate specific rows)

**Disadvantages**:
- More divisions/multiplications per element
- Slower than iterative DP approach
- Less intuitive for beginners

---

## 💻 Solution 3: Recursive Approach

**Complexity: O(2ⁿ) without memo / O(n²) with memo | O(n²) Space**

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        if (numRows == 0) return {};
        if (numRows == 1) return {{1}};
        
        // Get previous rows recursively
        vector<vector<int>> prevTriangle = generate(numRows - 1);
        vector<int> prevRow = prevTriangle.back();
        
        // Build current row
        vector<int> currentRow(numRows, 1);
        for (int i = 1; i < numRows - 1; i++) {
            currentRow[i] = prevRow[i - 1] + prevRow[i];
        }
        
        prevTriangle.push_back(currentRow);
        return prevTriangle;
    }
};
```

### 🔍 Explanation

This approach builds Pascal's Triangle recursively by getting the previous rows first, then adding the next row.

**Algorithm**:
1. Base cases: Return empty for 0 rows, `[[1]]` for 1 row
2. Recursively get the triangle with (numRows - 1) rows
3. Extract the last row from the previous triangle
4. Build the new row using the sum of adjacent elements
5. Add the new row to the triangle and return

**Advantages**:
- Demonstrates recursive thinking
- Clean separation of concerns

**Disadvantages**:
- Exponential time complexity without memoization
- Higher memory usage due to recursive call stack
- Slower than iterative approach
- Not suitable for production code

---

## 💻 Solution 4: Alternative Iterative with Binomial Coefficient

**Complexity: O(n²) Time | O(n²) Space**

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> triangle;
        
        for (int row = 1; row <= numRows; row++) {
            vector<int> ansRow;
            long long ans = 1;
            ansRow.push_back(1);
            
            for (int i = 1; i < row; i++) {
                ans *= (row - i);
                ans /= i;
                ansRow.push_back((int)ans);
            }
            
            triangle.push_back(ansRow);
        }
        
        return triangle;
    }
};
```

### 🔍 Explanation

This approach generates each row independently using the binomial coefficient formula optimized with incremental calculation.

**Algorithm**:
1. For each row from 1 to numRows
2. Start with the first element = 1
3. For each subsequent element, multiply by (row - i) and divide by i
4. Add all elements to form the current row
5. Add row to the triangle

**Key Insight**: C(n, k) = C(n, k-1) × (n-k+1) / k, allowing incremental calculation.

**Advantages**:
- More efficient than naive binomial coefficient
- Independent row calculation
- Good mathematical elegance

**Disadvantages**:
- Slightly less intuitive than pure DP
- More complex variable management

---

## 📊 Approach Comparison Table

| Aspect | Iterative DP | Binomial | Recursive | Alt. Binomial |
|--------|-------------|---------|-----------|---------------|
| **Time** | O(n²) | O(n² × k) | O(2ⁿ)/O(n²) memo | O(n²) |
| **Space** | O(n²) | O(n²) | O(n²) | O(n²) |
| **Code Clarity** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| **Production Use** | ✅ YES | ❌ No | ❌ No | ✅ YES |
| **Learning Value** | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| **Interview Ready** | ✅ YES | ✅ YES | ✅ YES | ✅ YES |

---

## 💡 Key Intuitions

### 🏔️ Mountain Building Analogy
Think of Pascal's Triangle like stacking stones to build a pyramid:
- **Foundation**: Start with a single stone at the top (row 0)
- **Layer Rules**: Each stone rests on two stones below (sum of two numbers)
- **Edge Stones**: Stones on the edges have only one stone below, so they're always 1
- **Building Up**: Each layer uses information from the layer directly below

### 🎯 Addition Grid Mental Model
Like a sliding window addition game:
1. Slide a 2-element window across the previous row
2. Add the two numbers in the window
3. Each sum becomes an element in the new row
4. Edges are always 1 (no addition needed)

### 🧩 Dynamic Programming Pattern
Pascal's Triangle demonstrates optimal substructure:
- **Subproblem**: Each row depends only on the previous row
- **Recurrence**: `triangle[i][j] = triangle[i-1][j-1] + triangle[i-1][j]`
- **Base Case**: First row is `[1]`
- **Bottom-Up**: Build from row 0 upward to row numRows-1

### 📊 Mathematical Properties
Pascal's Triangle has fascinating properties:
- **Binomial Coefficients**: Row n contains coefficients of (x+y)ⁿ expansion
- **Symmetry**: Each row is symmetric (palindromic)
- **Row Sum**: Sum of row n = 2ⁿ
- **Fibonacci**: Diagonal sums form Fibonacci sequence
- **Powers of 11**: First few rows are powers of 11 (11⁰=1, 11¹=11, 11²=121)

---

## ⚠️ Edge Cases & Robustness

### Edge Cases Handled

1. **Single Row**: `numRows = 1` → `[[1]]`
2. **Two Rows**: `numRows = 2` → `[[1],[1,1]]`
3. **Maximum Rows**: `numRows = 30` (all approaches handle efficiently)
4. **Small Triangle**: `numRows = 3` → `[[1],[1,1],[1,2,1]]`

### Algorithm Guarantees

✅ **Correct Edges**: All rows start and end with 1
✅ **Correct Inner Values**: Sum formula applied correctly
✅ **Proper Size**: Row i has exactly i+1 elements
✅ **No Overflow**: Values stay within int range for constraints
✅ **Sequential Dependencies**: Each row uses previous row correctly

### Robustness Features

- **Index Safety**: Proper bounds checking (1 to i-1)
- **Initialization**: All elements start as 1
- **Memory Efficient**: No extra buffers
- **Clean Code**: Uniform handling for all inputs
- **Cache Friendly**: Sequential memory access

---

## 🎓 Recommendation

**For Interviews & Production**: Use **Solution 1 (Optimal Iterative)**
- Cleanest implementation
- Best time/space complexity
- Easiest to explain
- Shows strong DP understanding

**For Learning Mathematics**: Use **Solution 2 or 4 (Binomial)**
- Connects to mathematical theory
- Great for understanding binomial coefficients

**For Understanding Recursion**: Use **Solution 3 (Recursive)**
- Shows recursive thinking
- Good for learning recurrence relations

---

**🎓 Learning Outcome**: This problem demonstrates the beauty of dynamic programming in its simplest form. Pascal's Triangle showcases how complex mathematical structures can be generated efficiently by building upon previous results. The triangle's connections to binomial coefficients, combinatorics, and algebra make it a fundamental concept in mathematics, while the algorithmic solutions teach us how to translate mathematical recurrence relations into efficient code.