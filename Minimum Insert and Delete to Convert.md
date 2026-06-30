# Minimum Insertion and Deletion Operations to Convert Array A to B

## Problem Overview & Reduction to LIS

The goal is to find the minimum number of insertions and deletions required to transform array `a[]` into `b[]`. Given that `b[]` contains **distinct** and **sorted** elements, we can optimize the traditional $\mathcal{O}(n \times m)$ Longest Common Subsequence (LCS) dynamic programming approach down to $\mathcal{O}(n \log m)$ by reducing it to the **Longest Increasing Subsequence (LIS)** problem.

### Algorithm Steps
1. **Index Mapping**: Create a hash map/dictionary of elements in `b[]` mapping each value to its corresponding index. Since `b[]` is sorted and elements are unique, these indices are strictly increasing (`0, 1, ..., m-1`).
2. **Coordinate Transformation**: Iterate through `a[]`. If an element exists in `b[]`, replace it with its mapped index from `b[]` and collect it into a new array `transformed_a`. Elements not present in `b[]` are ignored because they must be deleted anyway.
3. **Find the LIS**: Find the length of the Longest Increasing Subsequence ($L$) of `transformed_a` using a binary search approach ($\mathcal{O}(n \log m)$). 
4. **Calculate Final Operations**:
   - Total Deletions from `a[]` = $n - L$
   - Total Insertions into `a[]` = $m - L$
   - **Total Operations** = $(n - L) + (m - L) = n + m - 2L$

---

## 1. C++ (C++17)

### Code
```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
#include <algorithm>

class Solution {
public:
    int minInsAndDel(std::vector<int> &a, std::vector<int> &b) {
        int n = a.size();
        int m = b.size();
        
        // Step 1: Map each element of b to its index
        std::unordered_map<int, int> b_map;
        for (int i = 0; i < m; i++) {
            b_map[b[i]] = i;
        }
        
        // Step 2: Transform array a into its relative indices from b
        std::vector<int> transformed_a;
        for (int val : a) {
            if (b_map.find(val) != b_map.end()) {
                transformed_a.push_back(b_map[val]);
            }
        }
        
        // Step 3: Find LIS length using binary search (std::lower_bound)
        std::vector<int> lis;
        for (int x : transformed_a) {
            auto it = std::lower_bound(lis.begin(), lis.end(), x);
            if (it == lis.end()) {
                lis.push_back(x); // Append if x is greater than all elements in lis
            } else {
                *it = x; // Replace the first element >= x
            }
        }
        
        int L = lis.size();
        
        // Step 4: Minimum operations formula
        return n + m - 2 * L;
    }
};

```

### Line-by-Line Explanation

* **Lines 10-11**: Capture sizes of `a` ($n$) and `b` ($m$).
* **Lines 14-17**: Initialize an `unordered_map` to map each element in `b` to its index position. This enables $\mathcal{O}(1)$ average lookup verification.
* **Lines 20-25**: Process array `a`. For every element, if it exists in `b_map`, its corresponding target index is appended to `transformed_a`.
* **Lines 28-36**: Implement the Patience Sorting LIS method. `std::lower_bound` uses binary search to locate the first element in `lis` that is $\ge x$. If no such element exists (`it == lis.end()`), `x` extends the subsequence. Otherwise, it overwrites the value to maximize potential future matches.
* **Line 41**: Evaluates the total actions via the mathematical operation reduction rule.

---

## 2. Java (Java 21)

### Code

```java
import java.util.*;

class Solution {
    public int minInsAndDel(int[] a, int[] b) {
        int n = a.length;
        int m = b.length;
        
        // Step 1: Map each element of b to its index
        Map<Integer, Integer> bMap = new HashMap<>();
        for (int i = 0; i < m; i++) {
            bMap.put(b[i], i);
        }
        
        // Step 2: Transform array a into its relative indices from b
        List<Integer> transformedA = new ArrayList<>();
        for (int val : a) {
            if (bMap.containsKey(val)) {
                transformedA.add(bMap.get(val));
            }
        }
        
        // Step 3: Find LIS length using Collections.binarySearch
        List<Integer> lis = new ArrayList<>();
        for (int x : transformedA) {
            int idx = Collections.binarySearch(lis, x);
            
            // If x is not found, binarySearch returns: (-(insertion point) - 1)
            if (idx < 0) {
                idx = -(idx + 1);
            }
            
            if (idx == lis.size()) {
                lis.add(x);
            } else {
                lis.set(idx, x);
            }
        }
        
        int L = lis.size();
        
        // Step 4: Return total insertions and deletions
        return n + m - 2 * L;
    }
}

```

### Line-by-Line Explanation

* **Lines 10-13**: Loop through primitive array `b` and map values to indices inside a `HashMap<Integer, Integer>`.
* **Lines 16-21**: Convert the active matches from `a` into an `ArrayList` dynamically filtering unmapped elements using `.containsKey()`.
* **Lines 24-37**: Leverage `Collections.binarySearch()`. When an item is absent, Java returns a negative integer tracking the potential insertion index encoded as `-(insertion point) - 1`. We decode this back via `idx = -(idx + 1)`. If `idx` equals the size of the array, the subsequence expands. Otherwise, `.set(idx, x)` overwrites the larger bounding limit.

---

## 3. Python 3

### Code

```python
import bisect

class Solution:
    def minInsAndDel(self, a: list[int], b: list[int]) -> int:
        n = len(a)
        m = len(b)
        
        # Step 1: Map each element of b to its index using a dictionary comprehension
        b_map = {val: i for i, val in enumerate(b)}
        
        # Step 2: Transform array a filtering elements missing in b
        transformed_a = [b_map[val] for val in a if val in b_map]
        
        # Step 3: Find LIS length using the bisect module
        lis = []
        for x in transformed_a:
            idx = bisect.bisect_left(lis, x)
            if idx == len(lis):
                lis.append(x)
            else:
                lis[idx] = x
                
        L = len(lis)
        
        # Step 4: Minimum operations
        return n + m - 2 * L

```

### Line-by-Line Explanation

* **Line 9**: Uses Python list enumeration syntax and a dictionary comprehension to build a highly optimized key-value map for `b` in $\mathcal{O}(m)$.
* **Line 12**: Uses inline list comprehension filtering. Checks `val in b_map` and maps indices concurrently in an optimized C-level loop.
* **Lines 15-21**: Employs `bisect.bisect_left(lis, x)` which performs an exact $\mathcal{O}(\log L)$ left-biased lower-bound binary search. If the target index equals `len(lis)`, `lis.append(x)` grows the valid subarray; otherwise, `lis[idx] = x` updates it.

---

## 4. C#

### Code

```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public int minInsAndDel(int[] a, int[] b) {
        int n = a.Length;
        int m = b.Length;
        
        // Step 1: Map each element of b to its index
        Dictionary<int, int> bMap = new Dictionary<int, int>();
        for (int i = 0; i < m; i++) {
            bMap[b[i]] = i;
        }
        
        // Step 2: Transform array a based on b's sequence mappings
        List<int> transformedA = new List<int>();
        foreach (int val in a) {
            if (bMap.ContainsKey(val)) {
                transformedA.Add(bMap[val]);
            }
        }
        
        // Step 3: Find LIS length using List<T>.BinarySearch
        List<int> lis = new List<int>();
        foreach (int x in transformedA) {
            int idx = lis.BinarySearch(x);
            
            // If element is not found, BinarySearch returns the bitwise complement (~) of insertion point
            if (idx < 0) {
                idx = ~idx;
            }
            
            if (idx == lis.Count) {
                lis.Add(x);
            } else {
                lis[idx] = x;
            }
        }
        
        int L = lis.Count;
        
        // Step 4: Result computation
        return n + m - 2 * L;
    }
}

```

### Line-by-Line Explanation

* **Lines 10-13**: Iterates through array `b` storing values inside a type-safe generic `Dictionary<int, int>`.
* **Lines 16-21**: Evaluates valid configurations inside array `a` via `.ContainsKey()` and appends results to a `List<int>`.
* **Lines 24-37**: Utilizes `.BinarySearch()`. When an element is missing, C# optimization returns a negative complement value indicating where it belongs. Using the bitwise complement operator (`~idx`) decodes this value natively.

---

## 5. JavaScript (Node v22)

### Code

```javascript
class Solution {
    minInsAndDel(a, b) {
        const n = a.length;
        const m = b.length;
        
        // Step 1: Map each element of b to its index using native Map
        const bMap = new Map();
        for (let i = 0; i < m; i++) {
            bMap.set(b[i], i);
        }
        
        // Step 2: Transform array a into its relative indices from b
        const transformedA = [];
        for (let i = 0; i < n; i++) {
            if (bMap.has(a[i])) {
                transformedA.push(bMap.get(a[i]));
            }
        }
        
        // Step 3: Find LIS length using custom binary search helper
        const lis = [];
        
        const binarySearch = (target) => {
            let low = 0;
            let high = lis.length;
            while (low < high) {
                const mid = Math.floor((low + high) / 2);
                if (lis[mid] >= target) {
                    high = mid; // Narrow down upper bounds
                } else {
                    low = mid + 1; // Narrow down lower bounds
                }
            }
            return low;
        };

        for (const x of transformedA) {
            const idx = binarySearch(x);
            if (idx === lis.length) {
                lis.push(x);
            } else {
                lis[idx] = x;
            }
        }
        
        const L = lis.length;
        
        // Step 4: Total operations computation
        return n + m - 2 * L;
    }
}

```

### Line-by-Line Explanation

* **Lines 7-10**: Utilizes ECMAScript standard global `Map` instance to assign indices. Using a native `Map` rather than standard objects avoids stringifying key numbers.
* **Lines 13-18**: Iterates sequentially via an incremental counter block checking elements against `.has()` and pushing index representations via `.get()`.
* **Lines 23-35**: JavaScript lacks a standard sorting library binary finder. `binarySearch` implements a manual lower-bound closure. `Math.floor()` guarantees safe floating-point truncations during index mid-point configurations.

---

## Complexity Analysis

| Metric | Complexity | Description |
| --- | --- | --- |
| **Time Complexity** | $\mathcal{O}(n \log m + m)$ | Building the hash map takes $\mathcal{O}(m)$. Filtering array `a` and applying binary search LIS tracking takes $\mathcal{O}(n \log m)$ iterations since the search space maxes out at array `b`'s unique elements size ($m$). |
| **Space Complexity** | $\mathcal{O}(n + m)$ | Space allocation required for hash tracking structures and holding auxiliary array element references. |

