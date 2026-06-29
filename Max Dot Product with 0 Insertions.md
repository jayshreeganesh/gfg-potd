# Max Dot Product with 0 Insertions - Solutions

This document contains the Dynamic Programming solutions for the "Max Dot Product with 0 Insertions" problem in C++, Java, Python, C#, and JavaScript.

---

## Problem Approach

The problem allows inserting zeros into array `b` to match the length of array `a`. This is mathematically equivalent to selecting exactly `m` elements from array `a` (where `m` is the size of `b`) to pair with the elements of `b` in their original relative order. Any element in `a` that is skipped is effectively multiplied by an inserted `0`.

We use a 2D DP table `dp[i][j]` representing the maximum dot product using the first `i` elements of `a` and the first `j` elements of `b`. For each element, we have two choices:
1. **Take**: Match `a[i-1]` with `b[j-1]`. The value becomes `dp[i-1][j-1] + a[i-1] * b[j-1]`.
2. **Skip**: Match `a[i-1]` with an inserted `0`. The value is inherited from `dp[i-1][j]`. This is valid only if $i - 1 \ge j$.

---

## 1. C++ Solution

```cpp
class Solution {
public:
    int maxDotProduct(vector<int>& a, vector<int>& b) {
        int n = a.size();
        int m = b.size();
        
        // dp[i][j] stores the max dot product of first i elements of a and first j elements of b
        vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
        
        for (int j = 1; j <= m; j++) {
            for (int i = j; i <= n; i++) {
                int take = dp[i-1][j-1] + a[i-1] * b[j-1];
                int skip = (i - 1 >= j) ? dp[i-1][j] : 0;
                
                dp[i][j] = max(take, skip);
            }
        }
        
        return dp[n][m];
    }
};

```

---

## 2. Java Solution

```java
class Solution {
    public int maxDotProduct(int[] a, int[] b) {
        int n = a.length;
        int m = b.length;
        
        int[][] dp = new int[n + 1][m + 1];
        
        for (int j = 1; j <= m; j++) {
            for (int i = j; i <= n; i++) {
                int take = dp[i - 1][j - 1] + a[i - 1] * b[j - 1];
                int skip = (i - 1 >= j) ? dp[i - 1][j] : 0;
                
                dp[i][j] = Math.max(take, skip);
            }
        }
        
        return dp[n][m];
    }
}

```

---

## 3. Python 3 Solution

```python
class Solution:
    def maxDotProduct(self, a, b):
        n = len(a)
        m = len(b)
        
        dp = [[0] * (m + 1) for _ in range(n + 1)]
        
        for j in range(1, m + 1):
            for i in range(j, n + 1):
                take = dp[i-1][j-1] + a[i-1] * b[j-1]
                skip = dp[i-1][j] if (i - 1 >= j) else 0
                
                dp[i][j] = max(take, skip)
                
        return dp[n][m]

```

---

## 4. C# Solution

```csharp
using System;

public class Solution {
    public int maxDotProduct(int[] a, int[] b) {
        int n = a.Length;
        int m = b.Length;
        
        int[,] dp = new int[n + 1, m + 1];
        
        for (int j = 1; j <= m; j++) {
            for (int i = j; i <= n; i++) {
                int take = dp[i - 1, j - 1] + a[i - 1] * b[j - 1];
                int skip = (i - 1 >= j) ? dp[i - 1, j] : 0;
                
                dp[i, j] = Math.Max(take, skip);
            }
        }
        
        return dp[n, m];
    }
}

```

---

## 5. JavaScript (Node.js) Solution

```javascript
class Solution {
    maxDotProduct(a, b) {
        const n = a.length;
        const m = b.length;
        
        const dp = Array.from({ length: n + 1 }, () => Array(m + 1).fill(0));
        
        for (let j = 1; j <= m; j++) {
            for (let i = j; i <= n; i++) {
                const take = dp[i - 1][j - 1] + a[i - 1] * b[j - 1];
                const skip = (i - 1 >= j) ? dp[i - 1][j] : 0;
                
                dp[i][j] = Math.max(take, skip);
            }
        }
        
        return dp[n][m];
    }
}

```

---

## Complexity Analysis

* **Time Complexity:** $\mathcal{O}(n \times m)$ — Because of the nested loops bounded by the lengths of both arrays.
* **Space Complexity:** $\mathcal{O}(n \times m)$ — To maintain the 2D DP table. *(Note: This can be optimized to $\mathcal{O}(m)$ auxiliary space because each iteration only relies on the values from the previous row).*

