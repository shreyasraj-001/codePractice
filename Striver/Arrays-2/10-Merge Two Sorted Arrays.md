# Merge Two Sorted Arrays — [🔗 Problem Link:](https://www.geeksforgeeks.org/problems/merge-two-sorted-arrays-1587115620/1)

**Tags:** `Array`, `Two Pointers`, `Sorting`, `Gap Method`, `In-place Algorithm`

---

# 📌 Problem Statement

We are given **two sorted arrays** `a[]` and `b[]`.  
We must merge them such that the **combined elements remain sorted**, but **without using any extra space**.

Given two sorted arrays, rearrange them so that after merging:

- `a[]` contains the **first n smallest elements**
- `b[]` contains the **remaining elements**

**Constraints:**

* `1 ≤ n, m ≤ 10^5`
* `-10^9 ≤ values ≤ 10^9`
* Arrays are **already sorted**

```

Representative Example Test Case

Input
a = [1,4,7,8,10]
b = [2,3,9]

Output
a = [1,2,3,4,7]
b = [8,9,10]

Explanation
Both arrays together should behave like one sorted array:
[1,2,3,4,7,8,9,10]

Since no extra space is allowed, we rearrange elements
between arrays while maintaining sorted order.

```

---

# 🔎 Pattern Recognition

## Signals in the Problem

Clues that hint toward the pattern:

* **Two sorted arrays**
* **Merge requirement**
* **No extra space allowed**
* Need to maintain **global ordering**

Example signals:

* Sorted input
* Rearrangement without extra memory
* Cross-array comparisons
* In-place algorithm requirement

---

## Pattern Mapping

| Problem Type       | Technique             |
| ------------------ | --------------------- |
| Two sorted arrays  | Two Pointers          |
| In-place merge     | Gap Method (Shell Sort idea) |
| Memory restricted  | In-place swapping     |

---

```

Pattern Decision Tree

Sorted Arrays?
|
YES
|
Extra space allowed?
/ 
YES  NO
|     |
Merge     Gap Method
Sort

```

---

# 🧠 Key Observations

Important facts that simplify the solution:

* Arrays are **already sorted individually**
* Final merged order must be **globally sorted**
* We cannot use **O(n+m) auxiliary space**
* Elements may need to **move across arrays**

Key observation:

If we treat both arrays as **one virtual array**, we can compare elements that are **gap distance apart**.

---

# 💡 Core Intuition (Most Important Section)

The main challenge is merging **without extra memory**.

Normal merge sort uses:

```

O(n + m) extra space

```

But here we must do **in-place merging**.

Key trick:

Treat the arrays like a **single combined array**

```

[a1 a2 a3 ... an | b1 b2 b3 ... bm]

````

Then compare elements that are **gap distance apart**.

This idea comes from **Shell Sort**.

Steps:

1. Compare elements far apart first
2. Swap if out of order
3. Gradually reduce gap
4. Eventually gap becomes **1**, ensuring full sorting

This pushes larger elements toward the **right side** efficiently.

---

# 🧮 Approach 1 — Brute Force

## Idea

Merge both arrays into a **new temporary array**, sort it, and then split back.

---

## Steps

1. Create array of size `n+m`
2. Copy elements of both arrays
3. Sort the combined array
4. Copy first `n` elements into `a`
5. Copy remaining into `b`

---

## Java Implementation

```java
import java.util.*;

class Solution {
    public void mergeArrays(int a[], int b[]) {

        int n = a.length;
        int m = b.length;

        int[] temp = new int[n + m];

        int k = 0;

        for(int i = 0; i < n; i++)
            temp[k++] = a[i];

        for(int i = 0; i < m; i++)
            temp[k++] = b[i];

        Arrays.sort(temp);

        for(int i = 0; i < n; i++)
            a[i] = temp[i];

        for(int i = 0; i < m; i++)
            b[i] = temp[n + i];
    }
}
````

---

## Complexity

**Time Complexity:** `O((n+m) log(n+m))`
**Space Complexity:** `O(n+m)`

Problem with this approach:

* Uses **extra memory**, violating the constraint.

---

# 🧮 Approach 2 — Better Solution

## Idea

Compare the **largest element of a** with the **smallest element of b**.

If they are out of order, swap them and then **re-sort both arrays**.

---

## Steps

1. Use two pointers
2. Compare `a[n-1]` with `b[0]`
3. Swap if needed
4. Move pointers inward
5. Finally sort both arrays

---

## Java Implementation

```java
import java.util.*;

class Solution {
    public void mergeArrays(int a[], int b[]) {

        int n = a.length;
        int m = b.length;

        int left = n - 1;
        int right = 0;

        while(left >= 0 && right < m) {

            if(a[left] > b[right]) {

                int temp = a[left];
                a[left] = b[right];
                b[right] = temp;

                left--;
                right++;
            }
            else
                break;
        }

        Arrays.sort(a);
        Arrays.sort(b);
    }
}
```

---

## Complexity

**Time Complexity:** `O(n log n + m log m)`
**Space Complexity:** `O(1)`

Tradeoff:

* Avoids extra array but still requires **sorting again**.

---

# 🚀 Approach 3 — Optimal Solution

## Idea

Use the **Gap Method**, inspired by **Shell Sort**.

Instead of comparing adjacent elements, compare elements that are **gap distance apart**.

Gap starts as:

```
gap = ceil((n+m)/2)
```

Then gradually decreases until `gap = 1`.

---

## Steps

1. Treat both arrays as **one virtual array**
2. Initialize gap
3. Compare elements `gap` apart
4. Swap if out of order
5. Reduce gap using `ceil(gap/2)`
6. Stop when gap becomes `0`

---

## Java Implementation

```java
class Solution {

    public void mergeArrays(int a[], int b[]) {

        int n = a.length;
        int m = b.length;

        int len = n + m;

        int gap = (len / 2) + (len % 2);

        while (gap > 0) {

            int left = 0;
            int right = left + gap;

            while (right < len) {

                if (left < n && right < n) {

                    if (a[left] > a[right]) {
                        int temp = a[left];
                        a[left] = a[right];
                        a[right] = temp;
                    }
                }

                else if (left < n && right >= n) {

                    if (a[left] > b[right - n]) {
                        int temp = a[left];
                        a[left] = b[right - n];
                        b[right - n] = temp;
                    }
                }

                else {

                    if (b[left - n] > b[right - n]) {
                        int temp = b[left - n];
                        b[left - n] = b[right - n];
                        b[right - n] = temp;
                    }
                }

                left++;
                right++;
            }

            if (gap == 1)
                gap = 0;
            else
                gap = (gap / 2) + (gap % 2);
        }
    }
}
```

---

# 🔍 Dry Run Example

**Input**

```
a = [1,4,7,8,10]
b = [2,3,9]
```

Combined view

```
[1 4 7 8 10 | 2 3 9]
```

| Step | Variable / State | Value | Explanation               |
| ---- | ---------------- | ----- | ------------------------- |
| 1    | gap              | 4     | Compare elements 4 apart  |
| 2    | swap             | 4 ↔ 2 | Fix ordering              |
| 3    | swap             | 7 ↔ 3 | Move smaller element left |
| 4    | gap              | 2     | Reduce gap                |
| 5    | gap              | 1     | Final adjacent comparison |

**Final Answer →**

```
a = [1,2,3,4,7]
b = [8,9,10]
```

---

# ⚠️ Edge Cases

Consider:

* One array empty
* Arrays with duplicate elements
* Arrays already perfectly merged
* Arrays with negative numbers

Example:

```
a = [1,2,3]
b = []
```

Result remains unchanged.

---

# ⏱️ Complexity (Optimal)

**Time Complexity:** `O((n+m) log(n+m))`
**Space Complexity:** `O(1)`

---

# 🔁 Similar Problems

| Problem                          | Pattern       |
| -------------------------------- | ------------- |
| Merge Intervals                  | Sorting       |
| Merge Sorted Array (LeetCode 88) | Two Pointers  |
| Kth Element of Two Sorted Arrays | Binary Search |
| Median of Two Sorted Arrays      | Binary Search |

---

# 🎯 Interview Explanation

> “The brute force approach merges arrays using extra space.
> The improved approach swaps boundary elements and re-sorts arrays.
> The optimal solution uses the **Gap Method (Shell Sort idea)**, treating both arrays as a virtual array and gradually reducing the gap to achieve in-place merging with `O(1)` space.”

---

# 🚀 Advanced Section (Used by Top Candidates)

### 🔥 Pattern Tag

```
In-place Merge
Gap Method
Shell Sort Inspired
```

### 🧩 Pattern Family

```
In-place Sorting Algorithms
Two Pointer Techniques
Memory Optimized Merging
```
