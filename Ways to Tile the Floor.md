# Ways to Tile the Floor - Solutions by Language

This document contains the 1D Dynamic Programming solution for the "Ways to Tile the Floor" problem, written in all 5 languages supported on the page. All solutions calculate the number of ways to tile an $n \times m$ floor using $1 \times m$ tiles and return the result modulo $10^9+7$.

---

### 1. Javascript (Node v22)

```javascript
class Solution {
    countWays(n, m) {
        if (n < m) return 1;
        if (n === m) return 2;
        
        const mod = 1000000007;
        const dp = new Array(n + 1).fill(0);
        
        for (let i = 1; i < m; i++) {
            dp[i] = 1;
        }
        dp[m] = 2;
        
        for (let i = m + 1; i <= n; i++) {
            dp[i] = (dp[i - 1] + dp[i - m]) % mod;
        }
        
        return dp[n];
    }
}

```

---

### 2. C++ (17)

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    int countWays(int n, int m) {
        if (n < m) return 1;
        if (n == m) return 2;
        
        int mod = 1e9 + 7;
        vector<int> dp(n + 1, 0);
        
        for (int i = 1; i < m; i++) {
            dp[i] = 1;
        }
        dp[m] = 2;
        
        for (int i = m + 1; i <= n; i++) {
            dp[i] = (dp[i - 1] + dp[i - m]) % mod;
        }
        
        return dp[n];
    }
};

```

---

### 3. Java (21)

```java
class Solution {
    public int countWays(int n, int m) {
        if (n < m) return 1;
        if (n == m) return 2;
        
        int mod = 1000000007;
        int[] dp = new int[n + 1];
        
        for (int i = 1; i < m; i++) {
            dp[i] = 1;
        }
        dp[m] = 2;
        
        for (int i = m + 1; i <= n; i++) {
            dp[i] = (dp[i - 1] + dp[i - m]) % mod;
        }
        
        return dp[n];
    }
}

```

---

### 4. Python3

```python
class Solution:
    def countWays(self, n, m):
        if n < m: 
            return 1
        if n == m: 
            return 2
            
        mod = 10**9 + 7
        dp = [0] * (n + 1)
        
        for i in range(1, m):
            dp[i] = 1
        dp[m] = 2
        
        for i in range(m + 1, n + 1):
            dp[i] = (dp[i - 1] + dp[i - m]) % mod
            
        return dp[n]

```

---

### 5. C#

```csharp
public class Solution {
    public int countWays(int n, int m) {
        if (n < m) return 1;
        if (n == m) return 2;
        
        int mod = 1000000007;
        int[] dp = new int[n + 1];
        
        for (int i = 1; i < m; i++) {
            dp[i] = 1;
        }
        dp[m] = 2;
        
        for (int i = m + 1; i <= n; i++) {
            dp[i] = (dp[i - 1] + dp[i - m]) % mod;
        }
        
        return dp[n];
    }
}

```

```

```
