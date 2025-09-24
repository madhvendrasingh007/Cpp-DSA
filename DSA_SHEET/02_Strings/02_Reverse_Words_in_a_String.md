# 🔄 Reverse Words in a String

<div align="center">

[![Difficulty](https://img.shields.io/badge/Difficulty-🟡%20Medium-yellow?style=for-the-badge)](https://leetcode.com/problems/reverse-words-in-a-string/)
[![Topics](https://img.shields.io/badge/Topics-String%20|%20Two%20Pointers-blue?style=for-the-badge)](https://leetcode.com/problems/reverse-words-in-a-string/)
[![Practice](https://img.shields.io/badge/Practice-LeetCode-orange?style=for-the-badge)](https://leetcode.com/problems/reverse-words-in-a-string/)
[![Article](https://img.shields.io/badge/Article-Medium-black?style=for-the-badge)](https://medium.com/@sheefanaaz6417/151-reverse-words-in-a-string-leetcode-step-by-step-approach-90de235ea9ef)

</div>

---

## 📋 Problem Statement

Given an input string `s`, reverse the order of the words. A word is defined as a sequence of non-space characters. The words in `s` will be separated by at least one space. Return a string of the words in reverse order concatenated by a single space.

**Note**: `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

### Examples:

**Example 1:**
```
Input: s = "the sky is blue"
Output: "blue is sky the"
```

**Example 2:**
```
Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
```

**Example 3:**
```
Input: s = "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
```

### Constraints:
- `1 ≤ s.length ≤ 10⁴`
- `s` contains English letters (upper-case and lower-case), digits, and spaces `' '`
- There is **at least one** word in `s`

---

## 💻 C++ Implementation (Optimal Approach)

```cpp
class Solution {
public:
    string reverseWords(string s) {
        // Step 1: Remove leading, trailing, and extra spaces
        int n = s.length();
        int left = 0, right = 0;
        
        // Remove leading spaces and normalize multiple spaces
        while (right < n) {
            // Skip leading spaces
            while (right < n && s[right] == ' ') right++;
            
            // Copy non-space characters
            while (right < n && s[right] != ' ') {
                s[left++] = s[right++];
            }
            
            // Add single space after word (except for last word)
            while (right < n && s[right] == ' ') right++;
            if (right < n) s[left++] = ' ';
        }
        
        // Resize string to remove trailing characters
        s.resize(left);
        
        // Step 2: Reverse the entire cleaned string
        reverse(s.begin(), s.end());
        
        // Step 3: Reverse each individual word
        int start = 0;
        for (int i = 0; i <= s.length(); i++) {
            if (i == s.length() || s[i] == ' ') {
                // Reverse word from start to i-1
                reverse(s.begin() + start, s.begin() + i);
                start = i + 1;  // Next word starts after space
            }
        }
        
        return s;
    }
};
```

---

## 🔍 Detailed Explanation

The algorithm works in **three main phases**:

### Phase 1: **String Cleaning** 🧹
- Remove leading and trailing spaces
- Normalize multiple consecutive spaces to single spaces
- Use two pointers (`left`, `right`) to compact the string in-place

### Phase 2: **Global Reversal** 🔄
- Reverse the entire cleaned string
- This puts words in reverse order, but each word is also reversed

### Phase 3: **Word-by-Word Reversal** 🔤
- Iterate through the globally reversed string
- Reverse each individual word to restore correct character order
- Words are now in reverse order with correct spelling

**Example Trace**: `s = "  the sky is blue  "`

1. **After Cleaning**: `"the sky is blue"`
2. **After Global Reverse**: `"eulb si yks eht"`
3. **After Word Reversal**: `"blue is sky the"`

**Key Insight**: By reversing twice (globally, then word-by-word), we achieve the desired result: words in reverse order but with correct internal spelling.

---

## 📊 Algorithm + Pseudocode

### Algorithm Steps:
1. **Clean the string**: Remove extra spaces and normalize
2. **Reverse entire string**: All characters get reversed
3. **Reverse each word**: Restore correct character order within words
4. **Return result**: Words now in reverse order

### Pseudocode:
```
ALGORITHM ReverseWords(s)
BEGIN
    // Phase 1: Clean string
    left ← 0, right ← 0
    WHILE right < length(s) DO
        // Skip spaces
        WHILE right < length(s) AND s[right] = ' ' DO
            right ← right + 1
        END WHILE
        
        // Copy word characters
        WHILE right < length(s) AND s[right] ≠ ' ' DO
            s[left] ← s[right]
            left ← left + 1
            right ← right + 1
        END WHILE
        
        // Add single space if more words exist
        IF right < length(s) THEN
            s[left] ← ' '
            left ← left + 1
        END IF
    END WHILE
    
    // Resize string
    s ← s[0...left-1]
    
    // Phase 2: Reverse entire string
    REVERSE(s)
    
    // Phase 3: Reverse each word
    start ← 0
    FOR i ← 0 TO length(s) DO
        IF i = length(s) OR s[i] = ' ' THEN
            REVERSE(s[start...i-1])
            start ← i + 1
        END IF
    END FOR
    
    RETURN s
END
```

---

## 🎯 Visual Diagram

```
Input: "  the  sky is   blue  "
       ↓
   [Phase 1: String Cleaning]
       ↓
   "the sky is blue"
       ↓
   [Phase 2: Global Reversal]
       ↓
   "eulb si yks eht"
       ↓
   [Phase 3: Word-by-Word Reversal]
       ↓
   Step-by-step word reversal:
   
   "eulb si yks eht"
    ^^^^            → reverse "eulb" → "blue"
   "blue si yks eht"
         ^^         → reverse "si" → "is"  
   "blue is yks eht"
            ^^^     → reverse "yks" → "sky"
   "blue is sky eht"
                ^^^  → reverse "eht" → "the"
   
   Final: "blue is sky the"

[Visual Process Flow]

Original:  [ ] [the] [ ] [ ] [sky] [is] [ ] [ ] [ ] [blue] [ ] [ ]
            ↓ Remove extra spaces
Cleaned:   [the] [ ] [sky] [ ] [is] [ ] [blue]
            ↓ Reverse entire string  
Reversed:  [eulb] [ ] [si] [ ] [yks] [ ] [eht]
            ↓ Reverse each word
Final:     [blue] [ ] [is] [ ] [sky] [ ] [the]
```

---

## ⚡ Complexity Analysis

### Time Complexity: **O(n)**
- **Phase 1 (Cleaning)**: O(n) - single pass through string
- **Phase 2 (Global Reverse)**: O(n) - reverse entire string
- **Phase 3 (Word Reverse)**: O(n) - each character processed once
- **Overall**: O(n) where n is length of input string
- **Explanation**: Each character is processed a constant number of times

### Space Complexity: **O(1)**
- **In-place operations**: String cleaning and reversals done in-place
- **No extra data structures**: Only a few pointer variables used
- **Explanation**: We modify the input string directly without using additional space proportional to input size

**Note**: If we consider the output string as additional space, then space complexity becomes O(n), but typically output space is not counted in complexity analysis.

---

## 🚀 Approach Evolution: Brute Force to Optimal

### 1. **Brute Force Approach** - O(n) Time, O(n) Space
```cpp
// Using stringstream and vector to split and reverse
string reverseWords(string s) {
    stringstream ss(s);
    vector<string> words;
    string word;
    
    // Extract all words
    while (ss >> word) {
        words.push_back(word);
    }
    
    // Build result in reverse order
    string result = "";
    for (int i = words.size() - 1; i >= 0; i--) {
        result += words[i];
        if (i > 0) result += " ";
    }
    
    return result;
}
```

### 2. **Stack-Based Approach** - O(n) Time, O(n) Space
```cpp
// Using stack to reverse word order
string reverseWords(string s) {
    stack<string> wordStack;
    string word = "";
    
    // Parse and push words to stack
    for (char c : s) {
        if (c != ' ') {
            word += c;
        } else if (!word.empty()) {
            wordStack.push(word);
            word = "";
        }
    }
    if (!word.empty()) wordStack.push(word);
    
    // Pop from stack to get reverse order
    string result = "";
    while (!wordStack.empty()) {
        result += wordStack.top();
        wordStack.pop();
        if (!wordStack.empty()) result += " ";
    }
    
    return result;
}
```

### 3. **Optimal In-Place Approach** - O(n) Time, O(1) Space
```cpp
// Current solution: Three-phase in-place approach
string reverseWords(string s) {
    // Clean → Global Reverse → Word Reverse
    // (Implementation shown above)
}
```

**Evolution**: From using extra space for word storage to achieving the same result with constant extra space through clever in-place manipulations.

---

## 💡 Intuition Behind the Solution

### 🎭 **Theatre Analogy**
Think of words as **actors on a stage**:
- **Original**: Actors are lined up left to right
- **Goal**: Want actors in reverse order, but each actor facing forward
- **Solution**: 
  1. **Clean the stage** (remove extra spaces)
  2. **Flip entire stage** (all actors now face backward and in reverse order)
  3. **Each actor turns around** (now facing forward but still in reverse order)

### 🧩 **Puzzle Piece Mental Model**
1. **Problem**: Rearrange word puzzle pieces in reverse order
2. **Insight**: Instead of moving pieces individually, flip the entire puzzle
3. **Fix**: Flip each piece back to readable orientation
4. **Result**: Pieces in reverse order, each piece readable

### 🔄 **Double Reversal Principle**
- **Key Insight**: `reverse(reverse(word) + reverse(word2)) ≠ reverse(word1) + reverse(word2)`
- **But**: `reverse(reverse(reverse(word1 + word2))) = reverse(word1) + reverse(word2)` 
- **Application**: Reverse everything, then fix individual components

---

## ⚠️ Edge Cases & Considerations

### Edge Cases Handled:

1. **Leading Spaces**: `"  hello world"`
   - **Input**: `"  hello world"`
   - **Output**: `"world hello"`
   - **Handling**: Cleaning phase removes leading spaces

2. **Trailing Spaces**: `"hello world  "`
   - **Input**: `"hello world  "`
   - **Output**: `"world hello"`
   - **Handling**: String resize after cleaning removes trailing characters

3. **Multiple Spaces**: `"hello    world"`
   - **Input**: `"hello    world"`
   - **Output**: `"world hello"`
   - **Handling**: Normalization reduces multiple spaces to single space

4. **Single Word**: `"hello"`
   - **Input**: `"hello"`
   - **Output**: `"hello"`
   - **Handling**: Global reverse + word reverse = original word

5. **Single Character Words**: `"a b c"`
   - **Input**: `"a b c"`
   - **Output**: `"c b a"`
   - **Handling**: Each single character is its own reverse

### Robustness Features:
- ✅ **Handles all space variations**: Leading, trailing, multiple
- ✅ **Preserves word integrity**: Words maintain correct spelling
- ✅ **Memory efficient**: In-place operations minimize space usage
- ✅ **Single pass cleaning**: Efficient space normalization
- ✅ **Boundary handling**: Correctly processes string boundaries

### Algorithm Guarantees:
- **Output format**: Exactly one space between words
- **No extra spaces**: Leading/trailing spaces removed
- **Word preservation**: Internal word structure maintained
- **Order reversal**: Words appear in exactly reverse order

---

**🎓 Learning Outcome**: This problem demonstrates how **double reversal** can elegantly solve ordering problems while maintaining internal structure - a technique applicable to many string and array manipulation challenges.