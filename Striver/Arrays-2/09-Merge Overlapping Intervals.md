# Merge Overlapping Intervals [Problem Link](https://leetcode.com/problems/merge-intervals/)

**Tags:** `Array`, `Greedy`, `Sorting`, `Intervals`, `Pattern: Interval Merging`

---

# 📌 Problem Statement

Given a collection of intervals where each interval is represented as `[start, end]`, merge all overlapping intervals.

Return a list of intervals such that **no two intervals overlap**, and each merged interval represents the union of overlapping intervals.

**Constraints:**

* `1 ≤ intervals.length ≤ 10^4`
* `0 ≤ start ≤ end ≤ 10^4`
* Intervals may be unsorted.

## Example 1

```

Input:
intervals = [[1,3],[2,6],[8,10],[15,18]]

Output:
[[1,6],[8,10],[15,18]]

Explanation:
[1,3] and [2,6] overlap → merged to [1,6]

```
---

# 🔎 Pattern Recognition

## Signals in the Problem

Clues that hint toward the pattern:

* Input consists of **interval ranges**
* Need to **merge overlapping ranges**
* Intervals can appear **unsorted**
* Condition based on **start and end boundaries**
* Need to detect **overlapping segments**

Example signals:

* Problems involving `[start, end]`
* Need to combine overlapping segments
* Sorting helps simplify comparisons
* Adjacent interval comparisons

---

## Pattern Mapping

| Problem Type            | Technique             |
| ----------------------- | --------------------- |
| Overlapping intervals   | Sorting + Greedy      |
| Range merging problems  | Interval pattern      |
| Scheduling conflicts    | Interval comparison   |

---

Pattern Decision Tree

```

Does the problem contain ranges / intervals?
    YES
     |
Are intervals overlapping / need merging?
    YES
     |
Sort intervals by start → Merge sequentially

```

---

# 🧠 Key Observations

Important facts that simplify the solution:

* Overlap occurs when

```

next.start ≤ current.end

```

* If intervals are **sorted by start time**, overlap checks become easy.
* Only **adjacent intervals need to be compared**.
* Merging expands the current interval's end.

These observations reduce unnecessary comparisons.

---

# 💡 Core Intuition (Most Important Section)

The key idea is to **sort intervals by start time** and merge them greedily.

Why this works:

* After sorting, intervals appear in **left-to-right order**.
* If two intervals overlap, their ranges can be merged into one.
* If they do not overlap, the current interval is finalized and stored.

Key invariant:

```

current interval always represents the merged range so far

````

Thus the algorithm continuously **extends or stores intervals** depending on overlap.

---

# 🧮 Approach 1 — Brute Force

## Idea

Compare each interval with every other interval to detect overlap and merge.

Typical logic:

* Pick an interval
* Compare with every other interval
* Merge if overlapping

---

## Steps

1. Iterate through all intervals
2. For each interval compare with remaining intervals
3. Merge if overlapping
4. Track merged results

---

## Java Implementation

```java
class Solution {
    public int[][] merge(int[][] intervals) {

        int n = intervals.length;

        for(int i=0;i<n;i++){
            for(int j=i+1;j<n;j++){

                if(intervals[i][1] >= intervals[j][0]){

                    intervals[i][1] =
                        Math.max(intervals[i][1], intervals[j][1]);

                    intervals[j][0] = -1;
                    intervals[j][1] = -1;
                }
            }
        }

        List<int[]> res = new ArrayList<>();

        for(int[] interval : intervals){
            if(interval[0] != -1)
                res.add(interval);
        }

        return res.toArray(new int[res.size()][]);
    }
}
````

---

## Complexity

**Time Complexity:** `O(n²)`
**Space Complexity:** `O(n)`

Problem with this approach:

* Too many comparisons
* Inefficient for large inputs

---

# 🧮 Approach 2 — Better Solution

## Idea

Sort intervals first and then merge overlapping ones using nested comparison.

Sorting ensures intervals are processed sequentially.

---

## Steps

1. Sort intervals by start time
2. Traverse intervals
3. Compare current interval with next interval
4. Merge overlapping intervals
5. Stop merging when overlap ends

---

## Java Implementation

```java
import java.util.*;

class Solution {

    public static int[][] mergeOverlap(int[][] arr) {

        int n = arr.length;

        Arrays.sort(arr, (a,b) -> Integer.compare(a[0], b[0]));

        ArrayList<int[]> res = new ArrayList<>();

        for(int i=0;i<n;i++){

            int start = arr[i][0];
            int end = arr[i][1];

            if(!res.isEmpty() && res.get(res.size()-1)[1] >= end)
                continue;

            for(int j=i+1;j<n;j++){

                int fir = arr[j][0];
                int sec = arr[j][1];

                if(end >= fir)
                    end = Math.max(end, sec);
                else
                    break;
            }

            res.add(new int[]{start,end});
        }

        return res.toArray(new int[res.size()][]);
    }
}
```

---

## Complexity

**Time Complexity:** `O(n log n)`
**Space Complexity:** `O(n)`

Tradeoff:

* Sorting required
* Still contains nested traversal

---

# 🚀 Approach 3 — Optimal Solution

## Idea

Use **sorting + single traversal merging**.

Maintain a current interval and update it as long as overlap continues.

This removes the nested loop.

---

## Steps

1. Sort intervals by start
2. Maintain current interval
3. If next interval overlaps → merge
4. Else push current interval to result
5. Update current interval

---

## Java Implementation

```java
import java.util.*;

class Solution {

    public int[][] merge(int[][] intervals) {

        Arrays.sort(intervals, (a,b) -> a[0] - b[0]);

        List<int[]> res = new ArrayList<>();

        int[] current = intervals[0];

        for(int i=1;i<intervals.length;i++){

            if(current[1] >= intervals[i][0]){

                current[1] =
                    Math.max(current[1], intervals[i][1]);

            } else {

                res.add(current);
                current = intervals[i];
            }
        }

        res.add(current);

        return res.toArray(new int[res.size()][]);
    }
}
```

---

# 🔍 Dry Run Example

**Input**

```
[[1,3],[2,6],[8,10],[15,18]]
```

Sorted intervals:

```
[1,3] [2,6] [8,10] [15,18]
```

| Step | Variable / State | Value  | Explanation          |
| ---- | ---------------- | ------ | -------------------- |
| 1    | current          | [1,3]  | start first interval |
| 2    | compare          | [2,6]  | overlap detected     |
| 3    | merge            | [1,6]  | expand end           |
| 4    | compare          | [8,10] | no overlap           |
| 5    | store            | [1,6]  | finalized            |
| 6    | current          | [8,10] | new interval         |

**Final Answer →**

```
[[1,6],[8,10],[15,18]]
```

---

# ⚠️ Edge Cases

Consider:

* Only one interval
* Fully overlapping intervals
* Intervals touching at boundary `[1,4] [4,5]`
* Nested intervals `[1,10] [2,5]`

Example verification:

```java
if(intervals.length <= 1)
    return intervals;
```

---

# ⏱️ Complexity (Optimal)

**Time Complexity:** `O(n log n)`
(Sorting dominates)

**Space Complexity:** `O(n)`

---

# 🔁 Similar Problems

| Problem                   | Pattern          |
| ------------------------- | ---------------- |
| Insert Interval           | Interval Merging |
| Meeting Rooms             | Interval Overlap |
| Non Overlapping Intervals | Greedy           |
| Employee Free Time        | Interval Gap     |

---

# 🎯 Interview Explanation

> “The brute force approach checks all interval pairs. The improved solution sorts intervals and merges overlapping ranges using nested traversal. The optimal approach uses sorting and a single greedy traversal to merge intervals efficiently in `O(n log n)` time.”

---

# 🚀 Advanced Section (Used by Top Candidates)

### 🔥 Pattern Tag

```
Interval Merging
Greedy + Sorting Pattern
```

### 🚨 Common Mistake

```
Using start instead of end while merging

end = Math.max(end, start)   ❌
end = Math.max(end, nextEnd) ✅
```

### ⏱ Interview Recognition Time

```
~5–10 seconds
```

### 🧩 Pattern Family

```
Intervals / Range Processing / Greedy Algorithms
```

```
```
