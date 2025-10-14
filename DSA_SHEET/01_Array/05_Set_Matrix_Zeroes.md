# 🔢 Set Matrix Zeroes

<div align="center">

[![Difficulty](https://img.shields.io/badge/Difficulty-🟡%20Medium-yellow?style=for-the-badge)](https://leetcode.com/problems/set-matrix-zeroes/)
[![Topics](https://img.shields.io/badge/Topics-Array%20|%20Hash%20Table%20|%20Matrix-blue?style=for-the-badge)](https://leetcode.com/problems/set-matrix-zeroes/)
[![Practice](https://img.shields.io/badge/Practice-LeetCode-orange?style=for-the-badge)](https://leetcode.com/problems/set-matrix-zeroes/)
[![Article](https://img.shields.io/badge/Article-Medium-black?style=for-the-badge)](https://codeanddebug.in/blog/set-matrix-zeros/)

</div>

---

## 📋 Problem Statement

Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.

You must do it **in place**.

### Examples:

**Example 1:**
```
Input: matrix = [[1,1,1],[1,0,1],[1,1,1]]
Output: [[1,0,1],[0,0,0],[1,0,1]]
```
Visual:
```
1 1 1       1 0 1
1 0 1  -->  0 0 0
1 1 1       1 0 1
```

**Example 2:**
```
Input: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
Output: [[0,0,0,0],[0,4,5,0],[0,3,1,0]]
```
Visual:
```
0 1 2 0       0 0 0 0
3 4 5 2  -->  0 4 5 0
1 3 1 5       0 3 1 0
```

### Constraints:
- `m == matrix.length`
- `n == matrix[0].length`
- `1 ≤ m, n ≤ 200`
- `-2³¹ ≤ matrix[i][j] ≤ 2³¹ - 1`

### Follow-up:
- A straightforward solution using O(mn) space is probably a bad idea.
- A simple improvement uses O(m + n) space, but still not the best solution.
- Could you devise a constant space solution?

---

## 💻 C++ Implementation (Optimal Approach)

```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        
        // Use first row and column as markers
        bool firstRowZero = false;
        bool firstColZero = false;
        
        // Step 1: Check if first row contains any zero
        for (int j = 0; j < n; j++) {
            if (matrix[0][j] == 0) {
                firstRowZero = true;
                break;
            }
        }
        
        // Step 2: Check if first column contains any zero
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) {
                firstColZero = true;
                break;
            }
        }
        
        // Step 3: Mark zeros on first row and column based on inner matrix
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;  // Mark row
                    matrix[0][j] = 0;  // Mark column
                }
            }
        }
        
        // Step 4: Use markers to set zeros in inner matrix
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        
        // Step 5: Set first row to zero if needed
        if (firstRowZero) {
            for (int j = 0; j < n; j++) {
                matrix[0][j] = 0;
            }
        }
        
        // Step 6: Set first column to zero if needed
        if (firstColZero) {
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        }
    }
};
```

---

## 🔍 Detailed Explanation

The algorithm uses **first row and first column as markers** to achieve O(1) space complexity:

### 🔑 **Core Strategy**:
1. **Preserve First Row/Column Info**: Store whether first row/column should be zero
2. **Use as Markers**: Use first row and column to mark which rows/columns need zeroing
3. **Process Inner Matrix**: Set zeros based on markers
4. **Handle First Row/Column**: Finally set first row/column based on preserved info

### **Why This Works**:
- **Space Optimization**: Instead of using separate arrays, we reuse the matrix itself
- **Avoid Conflict**: Process first row/column separately to avoid losing marker information
- **Two-Pass Marking**: First mark, then apply to avoid immediate corruption

**Example Trace**: `matrix = [[1,1,1],[1,0,1],[1,1,1]]`

**Initial State:**
```
1 1 1
1 0 1
1 1 1
```

**Step 1-2: Check first row/column**
- firstRowZero = false (no zeros in first row)
- firstColZero = false (no zeros in first column)

**Step 3: Mark using first row/column**
- Found 0 at (1,1)
- Mark: matrix[1][0] = 0, matrix[0][1] = 0
```
1 0 1
0 0 1
1 1 1
```

**Step 4: Set zeros based on markers**
- Row 1 marked → Set row 1 to zeros
- Column 1 marked → Set column 1 to zeros
```
1 0 1
0 0 0
1 0 1
```

**Step 5-6: Handle first row/column**
- firstRowZero = false → No change
- firstColZero = false → No change

**Final Result:**
```
1 0 1
0 0 0
1 0 1
```

---

## 📊 Algorithm + Pseudocode

### Algorithm Steps:
1. **Preserve first row/column state**: Check if they contain zeros
2. **Mark using first row/column**: For zeros in inner matrix, mark corresponding first row/column
3. **Apply markers to inner matrix**: Set zeros based on markers
4. **Handle first row**: Set to zeros if originally contained zero
5. **Handle first column**: Set to zeros if originally contained zero

### Pseudocode:
```
ALGORITHM SetZeroes(matrix)
BEGIN
    m ← rows(matrix)
    n ← cols(matrix)
    firstRowZero ← false
    firstColZero ← false
    
    // Check first row
    FOR j ← 0 TO n-1 DO
        IF matrix[0][j] = 0 THEN
            firstRowZero ← true
        END IF
    END FOR
    
    // Check first column
    FOR i ← 0 TO m-1 DO
        IF matrix[i][0] = 0 THEN
            firstColZero ← true
        END IF
    END FOR
    
    // Mark zeros using first row and column
    FOR i ← 1 TO m-1 DO
        FOR j ← 1 TO n-1 DO
            IF matrix[i][j] = 0 THEN
                matrix[i][0] ← 0
                matrix[0][j] ← 0
            END IF
        END FOR
    END FOR
    
    // Set zeros based on markers
    FOR i ← 1 TO m-1 DO
        FOR j ← 1 TO n-1 DO
            IF matrix[i][0] = 0 OR matrix[0][j] = 0 THEN
                matrix[i][j] ← 0
            END IF
        END FOR
    END FOR
    
    // Handle first row
    IF firstRowZero THEN
        FOR j ← 0 TO n-1 DO
            matrix[0][j] ← 0
        END FOR
    END IF
    
    // Handle first column
    IF firstColZero THEN
        FOR i ← 0 TO m-1 DO
            matrix[i][0] ← 0
        END FOR
    END IF
END
```

---

## 🎯 Visual Diagram

```
Example: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]

Initial Matrix:
  0 1 2 3
0 [0 1 2 0]
1 [3 4 5 2]
2 [1 3 1 5]

Step 1-2: Check First Row/Column
firstRowZero = true  (0 at position [0][0])
firstColZero = true  (0 at position [0][0])

Step 3: Mark using first row/column
Found zeros at: (0,0) and (0,3)
After marking:
  0 1 2 3
0 [0 0 0 0] ← First row (markers)
1 [0 4 5 2]
2 [0 3 1 5]
  ↑
  First column (markers)

Step 4: Apply markers to inner matrix
- Row 0 marked → (already handled separately)
- Col 0 marked → Set column 0 (except [0][0])
- Col 3 marked → Set column 3

After applying:
  0 1 2 3
0 [0 0 0 0]
1 [0 4 5 0]
2 [0 3 1 0]

Step 5-6: Handle first row/column
- firstRowZero = true → Row 0 already zeros
- firstColZero = true → Column 0 already zeros

Final Matrix:
  0 1 2 3
0 [0 0 0 0]
1 [0 4 5 0]
2 [0 3 1 0]
```

---

## ⚡ Complexity Analysis

### Time Complexity: **O(m × n)**
- **First row/column check**: O(m + n)
- **Mark phase**: O(m × n) - iterate through all elements once
- **Apply phase**: O(m × n) - iterate through all elements again
- **First row/column update**: O(m + n)
- **Overall**: O(m × n) where m = rows, n = columns
- **Explanation**: Each element is visited a constant number of times

### Space Complexity: **O(1)**
- **Auxiliary variables**: Only two boolean variables (firstRowZero, firstColZero)
- **In-place markers**: Using existing matrix space for marking
- **No extra data structures**: No additional arrays or storage
- **Explanation**: Constant extra space regardless of matrix size

**Note**: This achieves the optimal space complexity by cleverly reusing the matrix itself for storing marker information.

---

## 🚀 Approach Evolution: Brute Force to Optimal

### 1. **Brute Force** - O((NxM)x(N + M)) + O(N*M), Time, O(m×n) Space
```cpp
// Create a copy of matrix and mark zeros
void markRow(vector<vector<int>> &matrix, int n, int m, int i) {
    // Mark entire row i with -1 (for non-zero elements)
    for (int j = 0; j < m; j++) {
        if (matrix[i][j] != 0) {
            matrix[i][j] = -1;
        }
    }
}

void markCol(vector<vector<int>> &matrix, int n, int m, int j) {
    // Mark entire column j with -1 (for non-zero elements)
    for (int i = 0; i < n; i++) {
        if (matrix[i][j] != 0) {
            matrix[i][j] = -1;
        }
    }
}

vector<vector<int>> zeroMatrix(vector<vector<int>> &matrix, int n, int m) {

    // Step 1: Mark rows and cols containing 0
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (matrix[i][j] == 0) {
                markRow(matrix, n, m, i);
                markCol(matrix, n, m, j);
            }
        }
    }

    // Step 2: Convert all -1 to 0
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (matrix[i][j] == -1) {
                matrix[i][j] = 0;
            }
        }
    }

    return matrix;
}

```
### 🧠 Why the -1 Trick Works**

Because if you directly set zeros:

- **Problem**: You’d accidentally spread zeros too far, marking extra rows/columns that weren’t originally affected.
- The -1 acts like a temporary flag
- “I should be turned into 0, but not yet — after all scanning is complete.”

### 2. **Better Approach (Using Arrays)** - O(m×n) Time, O(m+n) Space
```cpp
// Use separate arrays to track rows and columns
void setZeroes(vector<vector<int>>& matrix) {
    int m = matrix.size();
    int n = matrix[0].size();
    
    vector<bool> rows(m, false);
    vector<bool> cols(n, false);
    
    // Mark rows and columns containing zeros
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (matrix[i][j] == 0) {
                rows[i] = true;
                cols[j] = true;
            }
        }
    }
    
    // Set zeros based on marked rows and columns
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (rows[i] || cols[j]) {
                matrix[i][j] = 0;
            }
        }
    }
}
```

### 3. **Optimal Approach (Using First Row/Column)** - O(m×n) Time, O(1) Space
```cpp
// Current solution using first row and column as markers (shown above)
```

**Evolution**: From using extra space for copies to using auxiliary arrays, finally to reusing the matrix itself for constant space.

---

## 💡 Intuition Behind the Solution

### 🎯 **Marker Board Analogy**
Think of the matrix like a **game board with markers**:
- **Problem**: Need to remember which rows/columns to zero
- **Naive Solution**: Use separate scoreboard (extra space)
- **Clever Solution**: Use the board edges (first row/column) as the scoreboard
- **Trick**: Remember the edge's original state separately before using it as markers

### 🗺️ **Treasure Map Mental Model**
Like marking a **treasure map**:
1. **Survey**: Note if borders already have marks (preserve border state)
2. **Internal Marking**: For each treasure (zero) found, mark its row/column on borders
3. **Apply Marks**: Use border marks to determine what to zero
4. **Finalize Borders**: Apply original border state at the end

### 🏗️ **Construction Site Analogy**
Building demolition planning:
1. **Safety Check**: Protect the perimeter first (save first row/column state)
2. **Mark for Demolition**: Use perimeter as planning zone to mark buildings
3. **Execute Demolition**: Demolish based on perimeter marks
4. **Handle Perimeter**: Finally handle perimeter based on original state

---

## ⚠️ Edge Cases & Considerations

### Edge Cases Handled:

1. **Single Row Matrix**: `matrix = [[1,0,3]]`
   - **Input**: `[[1,0,3]]`
   - **Output**: `[[0,0,0]]`
   - **Handling**: firstRowZero flag properly handles single row case

2. **Single Column Matrix**: `matrix = [[1],[0],[3]]`
   - **Input**: `[[1],[0],[3]]`
   - **Output**: `[[0],[0],[0]]`
   - **Handling**: firstColZero flag properly handles single column case

3. **All Zeros**: `matrix = [[0,0],[0,0]]`
   - **Input**: `[[0,0],[0,0]]`
   - **Output**: `[[0,0],[0,0]]`
   - **Handling**: Correctly identifies and maintains all zeros

4. **No Zeros**: `matrix = [[1,2],[3,4]]`
   - **Input**: `[[1,2],[3,4]]`
   - **Output**: `[[1,2],[3,4]]`
   - **Handling**: Matrix remains unchanged

5. **Zero in First Row/Column**: `matrix = [[0,1],[2,3]]`
   - **Input**: `[[0,1],[2,3]]`
   - **Output**: `[[0,0],[0,3]]`
   - **Handling**: Flags correctly preserve and apply first row/column state

6. **Large Matrix with Sparse Zeros**: `matrix = [[1,1,...],[1,0,...],...]`
   - **Handling**: Efficiently marks only affected rows/columns

### Algorithm Robustness:
- ✅ **Boundary Safety**: Proper handling of first row and column
- ✅ **State Preservation**: Boolean flags prevent information loss
- ✅ **In-place Modification**: No data corruption during processing
- ✅ **Order Independence**: Marking phase doesn't interfere with application phase
- ✅ **Edge Case Coverage**: Works for all matrix dimensions and zero distributions

### Performance Characteristics:
- **Best Case**: O(m×n) when no zeros exist (full scan still required)
- **Worst Case**: O(m×n) when many zeros exist (same time complexity)
- **Memory Efficient**: Only 2 boolean variables regardless of size
- **Cache Friendly**: Sequential memory access patterns
- **Practical Efficiency**: Minimal overhead with constant space

### Why This Approach Works:
- **Space Optimization**: Cleverly reuses existing matrix storage
- **Separation of Concerns**: Handles first row/column separately to avoid conflicts
- **Two-Phase Design**: Marking and applying phases prevent premature updates
- **Mathematical Soundness**: Guarantees correct result through proper ordering

---

**🎓 Learning Outcome**: This problem demonstrates the power of **in-place algorithms** and **clever data structure reuse**. The key insight is recognizing that we can use parts of the input itself as auxiliary storage when we carefully preserve original states. This technique of using edges/boundaries as markers while preserving their state separately is applicable to many matrix manipulation problems and showcases how to optimize space complexity from O(m+n) to O(1) through creative thinking.