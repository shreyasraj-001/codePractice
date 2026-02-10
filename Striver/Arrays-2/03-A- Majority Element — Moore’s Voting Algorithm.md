# Majority Element — Moore’s Voting Algorithm
🔗 Problem Link: [ https://leetcode.com/problems/majority-element/]( https://leetcode.com/problems/majority-element/)

---
## 📌 Problem Statement
Given an integer array `nums` of size `n`, return the **majority element**.

The majority element is the element that appears **more than ⌊n / 2⌋ times**.  
You may assume that the majority element **always exists** in the array.


## 🧠 Key Observation
- There can be **only one** element that appears more than `n/2` times.
- All other elements combined appear **less than `n/2` times`.

This guarantee is the foundation of Moore’s Voting Algorithm.


## 💡 Core Intuition (Very Important)

Moore’s Voting Algorithm works on the idea of **pair cancellation**.

- If two different elements are paired → they cancel each other.
- Since the majority element appears **more than all others combined**, it **cannot be completely canceled**.
- Hence, the majority element will always survive till the end.

👉 The algorithm does NOT check frequency explicitly —  
the guarantee is **embedded in the logic itself**.

---

## 🧮 Algorithm Idea

Maintain two variables:
- `candidate` → possible majority element
- `count` → vote balance

### Rules:
1. If `count == 0`, choose the current element as `candidate`
2. If current element == `candidate`, increment `count`
3. Else, decrement `count`

---

## ✅ Java Implementation

```java
class Solution {
    public int majorityElement(int[] nums) {
        int count = 0;
        int candidate = 0;

        for (int num : nums) {
            // Step 1: pick a new candidate if count is zero
            if (count == 0) {
                candidate = num;
            }

            // Step 2: vote
            if (num == candidate) {
                count++;
            } else {
                count--;
            }
        }

        return candidate;
    }
}
````

---

## 🔍 Dry Run Example

Input:

```
nums = [2, 2, 1, 1, 1, 2, 2]
```

| Index | Current | Candidate | Count | Explanation        |
| ----- | ------- | --------- | ----- | ------------------ |
| 0     | 2       | 2         | 1     | count = 0 → pick 2 |
| 1     | 2       | 2         | 2     | same as candidate  |
| 2     | 1       | 2         | 1     | cancel             |
| 3     | 1       | 2         | 0     | cancel             |
| 4     | 1       | 1         | 1     | new candidate      |
| 5     | 2       | 1         | 0     | cancel             |
| 6     | 2       | 2         | 1     | new candidate      |

✅ Final Answer: `2`

---

## ❓ Why No Explicit `> n/2` Check?

Because the problem **guarantees** the existence of a majority element.

Mathematically:

* Let majority count = `M`
* Other elements count = `N - M`
* Given: `M > N/2`

Even after all cancellations:

```
Remaining majority = M - (N - M) > 0
```

➡️ The majority element **must survive**, so verification is unnecessary.

---

## ⚠️ When Is Verification Needed?

If the problem does **NOT** guarantee a majority element,
a second pass is required to verify the candidate’s frequency.

```java
int freq = 0;
for (int num : nums) {
    if (num == candidate) freq++;
}

if (freq > nums.length / 2) return candidate;
return -1;
```

---

## ⏱️ Complexity Analysis

* **Time Complexity:** `O(n)`
* **Space Complexity:** `O(1)`

Optimal and interview-preferred solution.

---

## 🔁 Similar Problems (Same Pattern)

* Majority Element II (more than `n/3` times)
* Elements appearing more than `n/k` times
* Boyer–Moore Voting variations
* Pair cancellation / greedy counting problems

---

## 🎯 Interview One-Liner

> “Since the majority element appears more than n/2 times, it cannot be completely canceled by other elements. Moore’s Voting Algorithm leverages this fact using pairwise cancellation, guaranteeing the final candidate is the majority element.”

---


