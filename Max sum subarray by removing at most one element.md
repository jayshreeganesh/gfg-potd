Here are the complete, ready-to-submit implementations of the optimal $\mathcal{O}(n)$ time and $\mathcal{O}(n)$ space solution across all 5 programming languages for the [Max sum subarray by removing at most one element](https://www.geeksforgeeks.org/problems/max-sum-subarray-by-removing-at-most-one-element/1) problem, followed by a comprehensive architectural and line-by-line explanation.

---

### Complete Source Code for All 5 Languages

#### 1. C++ (C++17)

```cpp
class Solution {
  public:
    int maxSumSubarray(vector<int>& arr) {
        int n = arr.size();
        if (n == 1) return arr[0];
        
        vector<int> fw(n), bw(n);
        
        // 1. Forward Kadane's Pass
        int cur_max = arr[0];
        int max_so_far = arr[0];
        fw[0] = arr[0];
        for (int i = 1; i < n; i++) {
            cur_max = max(arr[i], cur_max + arr[i]);
            max_so_far = max(max_so_far, cur_max);
            fw[i] = cur_max;
        }
        
        // 2. Backward Kadane's Pass
        bw[n - 1] = arr[n - 1];
        cur_max = arr[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            cur_max = max(arr[i], cur_max + arr[i]);
            bw[i] = cur_max;
        }
        
        // 3. Dynamic Element Omission Evaluator
        int overall_max = max_so_far;
        for (int i = 1; i < n - 1; i++) {
            overall_max = max(overall_max, fw[i - 1] + bw[i + 1]);
        }
        
        return overall_max;
    }
};

```

#### 2. Java (Java 21)

```java
class Solution {
    public int maxSumSubarray(int[] arr) {
        int n = arr.length;
        if (n == 1) return arr[0];

        int[] fw = new int[n];
        int[] bw = new int[n];

        // 1. Forward Kadane's Pass
        int curMax = arr[0];
        int maxSoFar = arr[0];
        fw[0] = arr[0];
        for (int i = 1; i < n; i++) {
            curMax = Math.max(arr[i], curMax + arr[i]);
            maxSoFar = Math.max(maxSoFar, curMax);
            fw[i] = curMax;
        }

        // 2. Backward Kadane's Pass
        bw[n - 1] = arr[n - 1];
        curMax = arr[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            curMax = Math.max(arr[i], curMax + arr[i]);
            bw[i] = curMax;
        }

        // 3. Dynamic Element Omission Evaluator
        int overallMax = maxSoFar;
        for (int i = 1; i < n - 1; i++) {
            overallMax = Math.max(overallMax, fw[i - 1] + bw[i + 1]);
        }

        return overallMax;
    }
}

```

#### 3. Python 3

```python
class Solution:
    def maxSumSubarray(self, arr: list[int]) -> int:
        n = len(arr)
        if n == 1:
            return arr[0]
            
        fw = [0] * n
        bw = [0] * n
        
        # 1. Forward Kadane's Pass
        cur_max = arr[0]
        max_so_far = arr[0]
        fw[0] = arr[0]
        for i in range(1, n):
            cur_max = max(arr[i], cur_max + arr[i])
            max_so_far = max(max_so_far, cur_max)
            fw[i] = cur_max
            
        # 2. Backward Kadane's Pass
        bw[n - 1] = arr[n - 1]
        cur_max = arr[n - 1]
        for i in range(n - 2, -1, -1):
            cur_max = max(arr[i], cur_max + arr[i])
            bw[i] = cur_max
            
        # 3. Dynamic Element Omission Evaluator
        overall_max = max_so_far
        for i in range(1, n - 1):
            overall_max = max(overall_max, fw[i - 1] + bw[i + 1])
            
        return overall_max

```

#### 4. C#

```csharp
using System;

public class Solution {
    public int maxSumSubarray(int[] arr) {
        int n = arr.Length;
        if (n == 1) return arr[0];

        int[] fw = new int[n];
        int[] bw = new int[n];

        // 1. Forward Kadane's Pass
        int curMax = arr[0];
        int maxSoFar = arr[0];
        fw[0] = arr[0];
        for (int i = 1; i < n; i++) {
            curMax = Math.Max(arr[i], curMax + arr[i]);
            maxSoFar = Math.Max(maxSoFar, curMax);
            fw[i] = curMax;
        }

        // 2. Backward Kadane's Pass
        bw[n - 1] = arr[n - 1];
        curMax = arr[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            curMax = Math.Max(arr[i], curMax + arr[i]);
            bw[i] = curMax;
        }

        // 3. Dynamic Element Omission Evaluator
        int overallMax = maxSoFar;
        for (int i = 1; i < n - 1; i++) {
            overallMax = Math.Max(overallMax, fw[i - 1] + bw[i + 1]);
        }

        return overallMax;
    }
}

```

#### 5. JavaScript (Node v22)

```javascript
class Solution {
    maxSumSubarray(arr) {
        const n = arr.length;
        if (n === 1) return arr[0];

        const fw = new Array(n).fill(0);
        const bw = new Array(n).fill(0);

        // 1. Forward Kadane's Pass
        let curMax = arr[0];
        let maxSoFar = arr[0];
        fw[0] = arr[0];
        for (let i = 1; i < n; i++) {
            curMax = Math.max(arr[i], curMax + arr[i]);
            maxSoFar = Math.max(maxSoFar, curMax);
            fw[i] = curMax;
        }

        // 2. Backward Kadane's Pass
        bw[n - 1] = arr[n - 1];
        curMax = arr[n - 1];
        for (let i = n - 2; i >= 0; i--) {
            curMax = Math.max(arr[i], curMax + arr[i]);
            bw[i] = curMax;
        }

        // 3. Dynamic Element Omission Evaluator
        let overallMax = maxSoFar;
        for (let i = 1; i < n - 1; i++) {
            overallMax = Math.max(overallMax, fw[i - 1] + bw[i + 1]);
        }

        return overallMax;
    }
}

```

---

### Detailed Line-by-Line Breakdown & Architectural Strategy

#### Core Algorithmic Concept

Standard Kadane's algorithm computes the maximum contiguous subarray ending at index `i` using a single pass. However, when allowed to skip **at most one element**, a modification is needed. If we decide to drop an element at index `i`, the maximum possible contiguous subarray surrounding it becomes the maximum subarray ending right before it (`i-1`) added to the maximum subarray starting right after it (`i+1`).

To capture this efficiently without recalculating overlapping subproblems repeatedly, we use a **Two-Directional Dynamic Programming Array Split strategy**.

---

#### Step 1: Handling Base Constraints & Memory Allocation

* **Array Bounds Check:** We extract the size of the array `n`. If the input has only one element (`n == 1`), no removal can happen because the resulting subarray must remain non-empty. We instantly return `arr[0]`.
* **DP Matrix Initialization:** We construct two tracking arrays:
* `fw` (Forward): `fw[i]` caches the maximum subarray sum that concludes precisely at index `i`.
* `bw` (Backward): `bw[i]` caches the maximum subarray sum that initiates precisely at index `i`.



#### Step 2: Forward Kadane's Pass (Left-to-Right Execution)

* **Seeding State Variables:** We initialize `curMax` (local runner) and `maxSoFar` (global baseline winner) using `arr[0]`. `fw[0]` is set directly to `arr[0]`.
* **Sequential Processing Loop:** We sweep from index `1` up to `n - 1`. At each index `i`:
* **Dynamic Selection:** We evaluate `Math.max(arr[i], curMax + arr[i])`. This determines if it is more profitable to extend the existing contiguous string or completely dump previous values and restart fresh from `arr[i]`.
* **Global Record Logging:** `maxSoFar` is continuously tracked against `curMax` to log the all-time high subarray sum assuming **no elements are removed**.
* **Caching State:** We store the calculated `curMax` into `fw[i]` for later indexing during the splice phase.



#### Step 3: Backward Kadane's Pass (Right-to-Left Execution)

* **Re-seeding State Variables:** We shift execution to the back of the collection. The final array item `arr[n - 1]` is loaded into `bw[n - 1]` and our tracking variable `curMax` is reset to this boundary condition value.
* **Reverse Loops:** We work backward from index `n - 2` down to `0`. At each position `i`:
* **Reverse Choice Optimization:** We check `Math.max(arr[i], curMax + arr[i])`. This checks if extending a trailing contiguous component to the right yields a better sum than restarting a new subarray from index `i`.
* **Caching State:** The resulting reverse optimal peak value is saved directly into `bw[i]`.



#### Step 4: Splice Evaluation & Merging Subproblems

* **Baseline Set:** We initialize our returning variable `overallMax` to `maxSoFar` to establish a safe default result for scenarios where dropping an element offers no benefit.
* **The Drop Element Sweep:** We run a loop targeting indices from `1` to `n - 2` (skipping boundary ends since dropping endpoints reduces back to standard Kadane's calculations). For each candidate index `i`:
* **Bridging Subarrays:** By hypothetically omitting `arr[i]`, we connect the maximum forward-moving window coming up to index `i - 1` (`fw[i - 1]`) directly with the backward-moving window starting at index `i + 1` (`bw[i + 1]`).
* **Max Filter Check:** We compare this combined bridge sum against the existing `overallMax` using a maximum statement, maintaining the higher sum.


* **Return Terminal State:** `overallMax` is safely processed and returned.

---

### Complexity Matrix

| Metric | Complexity | Explanation |
| --- | --- | --- |
| **Time Complexity** | $\mathcal{O}(n)$ | The algorithm uses three independent linear loops across the array of length $n$. Since $3n \propto n$, the execution scales linearly, comfortably matching the strict time limits of arrays containing up to $10^6$ items. |
| **Space Complexity** | $\mathcal{O}(n)$ | Two auxiliary linear arrays (`fw` and `bw`) of size $n$ are reserved to store the intermediate DP memoized calculations. |
