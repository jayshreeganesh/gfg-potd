
# Check Subset Sum Divisible by K

Given an array `arr[]` of integers and a value `k`, return `true` if the sum of any non-empty subset of the given array is divisible by `k`. Otherwise, return `false`.

---

## 💡 Core Algorithmic Insights

To solve this problem efficiently within the given constraints, we combine two powerful techniques:

### 1. The Pigeonhole Principle Optimization
If the size of the array $n$ is greater than or equal to $k$ ($n \geq k$), we can immediately return `true` in $\mathcal{O}(1)$ time. 
* **Why?** Consider the prefix sums of the array: $P_1, P_2, \dots, P_n$. There are $n$ prefix sums. When we take these sums modulo $k$, there are only $k$ possible remainders ($0$ to $k-1$). 
* If any prefix sum has a remainder of $0$, that prefix itself forms a valid subarray divisible by $k$.
* If no prefix sum has a remainder of $0$, we have $n$ prefix sums distributed among $k-1$ remaining slots ($1$ to $k-1$). Since $n \geq k > k-1$, by the **Pigeonhole Principle**, at least two prefix sums must share the exact same remainder modulo $k$ ($P_i \equiv P_j \pmod k$). 
* The difference between these two prefixes ($P_j - P_i$) represents a contiguous subarray whose sum is perfectly divisible by $k$. Because a subarray is a valid subset, a solution is guaranteed.

### 2. Dynamic Programming (When $n < k$)
When $n < k$, we track all possible subset sum remainders using a boolean array `dp` of size $k$. 
* `dp[i]` is set to `true` if a subset sum yields a remainder of `i` when divided by `k`.
* For every element `x` in the array, we transition to a new state `next_dp` by combining `x` with all previously discovered remainders: `(i + (x % k)) % k`.
* If `dp[0]` becomes `true` at any point, a valid subset has been found.

---

## 1. C++ (17) Solution

### Code
```cpp
class Solution {
public:
    bool divisibleByK(vector<int>& arr, int k) {
        int n = arr.size();
        
        // 1. Pigeonhole Principle optimization
        if (n >= k) {
            return true;
        }
        
        // 2. dp[i] is true if a subset exists with (sum % k) == i
        vector<bool> dp(k, false);
        
        for (int x : arr) {
            int rem = x % k;
            
            // Handle negative values to ensure a positive index range [0, k-1]
            if (rem < 0) {
                rem += k;
            }
            
            // 3. Early exit if the element itself is divisible by k
            if (rem == 0) {
                return true;
            }
            
            // 4. Clone current states to avoid mixing mutations within the same iteration
            vector<bool> next_dp = dp;
            next_dp[rem] = true; // Current element can start a new subset
            
            // 5. Generate new remainders by combining x with existing subset sums
            for (int i = 0; i < k; i++) {
                if (dp[i]) {
                    next_dp[(i + rem) % k] = true;
                }
            }
            
            // 6. Efficiently update state using move semantics
            dp = move(next_dp);
            
            // 7. Check if a valid subset sum divisible by k has been reached
            if (dp[0]) {
                return true;
            }
        }
        
        return false;
    }
};

```

### Line-by-Line Explanation

* **Line 4-7:** Captures array size. If $n \geq k$, it applies the Pigeonhole Principle to return `true` instantly, eliminating heavy execution steps.
* **Line 10:** Allocates a boolean vector `dp` of size `k` initialized to `false` to store reachable remainders.
* **Line 12-17:** Iterates over every number `x`. The modulo operation is normalized `if (rem < 0)` to guarantee the index sits correctly within bounds.
* **Line 20-21:** Quick optimization—if any single element is natively divisible by `k`, it constitutes a valid non-empty subset of size 1.
* **Line 24-25:** Copies the existing `dp` table to `next_dp` and marks `next_dp[rem] = true`, reflecting that the element `x` can start a brand-new subset.
* **Line 28-32:** Iterates through all possible remainders `0` to `k-1`. If a remainder `i` was reachable (`dp[i] == true`), adding the current remainder forms a new valid remainder state at `(i + rem) % k`.
* **Line 35:** Uses `std::move` to swap pointers efficiently without reallocating or deeply copying memory structures.
* **Line 38-40:** Evaluates if a total modular balance of `0` (`dp[0]`) has been successfully reached, bubbling out an early `true`.

---

## 2. Java (21) Solution

### Code

```java
class Solution {
    public boolean divisibleByK(int[] arr, int k) {
        int n = arr.length;
        
        // 1. Pigeonhole Principle optimization
        if (n >= k) {
            return true;
        }
        
        // 2. dp[i] tracks if a subset sum % k == i exists
        boolean[] dp = new boolean[k];
        
        for (int x : arr) {
            int rem = x % k;
            
            // Normalize negative remainders to positive equivalent indices
            if (rem < 0) {
                rem += k;
            }
            
            // 3. Early exit if single element matches condition
            if (rem == 0) {
                return true;
            }
            
            // 4. Clone current snapshot to process separate transitions
            boolean[] nextDp = dp.clone();
            nextDp[rem] = true;
            
            // 5. Update reachability transitions
            for (int i = 0; i < k; i++) {
                if (dp[i]) {
                    nextDp[(i + rem) % k] = true;
                }
            }
            
            dp = nextDp;
            
            // 6. Early exit check
            if (dp[0]) {
                return true;
            }
        }
        
        return false;
    }
}

```

### Line-by-Line Explanation

* **Line 3-6:** Validates array length against `k` to leverage $\mathcal{O}(1)$ Pigeonhole exit paths.
* **Line 9:** Instantiates a primitive boolean array mapping possible remainders.
* **Line 12-16:** Extracts the remainder of element `x`. Java's `%` operator preserves signs, so `rem += k` correctly converts negative remainders into positive indices.
* **Line 19-21:** Returns true immediately if `rem == 0`.
* **Line 24:** Uses the primitive `.clone()` method to fast-copy current configurations into a buffer snapshot array.
* **Line 28-32:** Scans through the old `dp` layer. If a state index `i` is active, it flags the new offset position `(i + rem) % k` on the buffer layout.
* **Line 35:** Shifts the reference pointer of `dp` to target `nextDp`.
* **Line 38:** Checks if a perfectly divisible subset path has manifested at index `0`.

---

## 3. Python 3 Solution

### Code

```python
class Solution:
    def divisibleByK(self, arr: list[int], k: int) -> bool:
        n = len(arr)
        
        # 1. Pigeonhole Principle optimization
        if n >= k:
            return True
            
        # 2. Initialize DP table tracker
        dp = [False] * k
        
        for x in arr:
            # Python's % operator naturally returns a positive remainder
            rem = x % k
            
            # 3. Quick structural validation
            if rem == 0:
                return True
                
            # 4. Clone states using shallow list conversion
            next_dp = list(dp)
            next_dp[rem] = True
            
            # 5. Process state updates
            for i in range(k):
                if dp[i]:
                    next_dp[(i + rem) % k] = True
                    
            dp = next_dp
            
            # 6. Check for valid solution state
            if dp[0]:
                return True
                
        return False

```

### Line-by-Line Explanation

* **Line 5-6:** Directly tests length logic constraints using `len()`.
* **Line 9:** Creates a flat boolean array of size `k` using list multiplication `[False] * k`.
* **Line 13:** Python's modulo floor division behavior inherently resolves negative numbers into positive offsets (e.g., `-1 % 5` yields `4`), removing manual checks.
* **Line 16-17:** Terminates early if any item resolves directly to `0`.
* **Line 20-21:** Clones array state using `list(dp)` to isolate changes, assigning the base remainder index to `True`.
* **Line 24-26:** Scans the active configuration space, transforming active combinations into updated modular remainder positions.
* **Line 28:** Updates reference pointer directly.
* **Line 31-32:** Validates index position `0` for an early true evaluation status.

---

## 4. C# Solution

### Code

```csharp
public class Solution {
    public bool divisibleByK(int[] arr, int k) {
        int n = arr.Length;
        
        // 1. Pigeonhole Principle optimization
        if (n >= k) {
            return true;
        }
        
        // 2. Initialize DP tracking array
        bool[] dp = new bool[k];
        
        foreach (int x in arr) {
            int rem = x % k;
            
            // Keep negative remainder boundaries positive
            if (rem < 0) {
                rem += k;
            }
            
            // 3. Verify single element status
            if (rem == 0) {
                return true;
            }
            
            // 4. Create shallow copy snapshot
            bool[] nextDp = (bool[])dp.Clone();
            nextDp[rem] = true;
            
            // 5. Update modulo combinations across the table
            for (int i = 0; i < k; i++) {
                if (dp[i]) {
                    nextDp[(i + rem) % k] = true;
                }
            }
            
            dp = nextDp;
            
            // 6. Check if target remains cleanly satisfied
            if (dp[0]) {
                return true;
            }
        }
        
        return false;
    }
}

```

### Line-by-Line Explanation

* **Line 5-7:** Computes if `arr.Length >= k` to trigger an instant short-circuit escape.
* **Line 10:** Sets up the fixed-size array structure mapping `k` state indices.
* **Line 12-17:** Loops with `foreach` blocks. Corrects negative remainder offsets to keep the indexed target layout positive.
* **Line 20-22:** Returns `true` if the item natively satisfies the target divisor.
* **Line 25-26:** Executes a typed cast preservation via `(bool[])dp.Clone()` to duplicate state configurations cleanly.
* **Line 29-33:** Iterates across historical tracks, computing lookup references dynamically.
* **Line 36:** Redirects state reference handles.
* **Line 39-41:** Identifies whether the zero-remainder boundary has been hit.

---

## 5. JavaScript (Node v22) Solution

### Code

```javascript
class Solution {
    /**
     * @param {number[]} arr
     * @param {number} k
     * @returns {boolean}
     */
    divisibleByK(arr, k) {
        const n = arr.length;
        
        // 1. Pigeonhole Principle optimization
        if (n >= k) {
            return true;
        }
        
        // 2. Allocate DP state table tracker
        let dp = new Array(k).fill(false);
        
        for (const x of arr) {
            let rem = x % k;
            
            // Adjust negative remainders safely
            if (rem < 0) {
                rem += k;
            }
            
            // 3. Early check optimization
            if (rem === 0) {
                return true;
            }
            
            // 4. Deep copy snapshot via array spread
            let nextDp = [...dp];
            nextDp[rem] = true;
            
            // 5. Build dynamic subset variations
            for (let i = 0; i < k; i++) {
                if (dp[i]) {
                    nextDp[(i + rem) % k] = true;
                }
            }
            
            dp = nextDp;
            
            // 6. Evaluate target condition success
            if (dp[0]) {
                return true;
            }
        }
        
        return false;
    }
}

```

### Line-by-Line Explanation

* **Line 9-11:** Verifies the `length` property against `k` to apply the Pigeonhole optimization.
* **Line 14:** Allocates a standard contiguous array structure filled explicitly with `false` states via `.fill()`.
* **Line 16-21:** Iterates with `for...of`. Normalizes negative remainders using custom sign adjustments to prevent out-of-bounds exceptions.
* **Line 24-26:** Handles matching values quickly before tracking subset mutations.
* **Line 29-30:** Uses the high-performance spread operator `[...dp]` to create a copy of the state array.
* **Line 33-37:** Updates reachable remainders by shifting valid existing states by the current remainder.
* **Line 40:** Reassigns the reference variable to update the DP state.
* **Line 43-45:** Confirms if index `0` evaluates to `true`.

---

## ⏱️ Complexity Matrix

| Metric | Complexity | Explanation |
| --- | --- | --- |
| **Time Complexity** | $\mathcal{O}(k^2)$ | If $n \geq k$, the solution takes $\mathcal{O}(1)$ time. If $n < k$, the outer loop runs at most $k$ times, and the inner state transition loop runs exactly $k$ times. Thus, worst-case execution is bounded by $n \times k < k^2$. |
| **Space Complexity** | $\mathcal{O}(k)$ | Space is determined by the size of the auxiliary state tables (`dp` and `next_dp`), which scale linearly with the value of `k`. |

