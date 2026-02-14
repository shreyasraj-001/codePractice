# 🔷 Problem: [Majority Element II LeetCode 229](https://leetcode.com/problems/majority-element-ii/)

**Goal:**
Find all elements that appear **more than ⌊ n/3 ⌋ times**.

Example:

```
Input: [3,2,3]
Output: [3]
```

---

# 🧠 Step 1: Understand the Mathematical Constraint (Core Insight)

If an element appears more than **n/3 times**, how many such elements can exist?

Let’s assume there are 3 such elements:

Each appears > n/3 times.

Total count would be:

```
> n/3 + n/3 + n/3 = > n
```

Impossible ❌

👉 Therefore:

> At most **2 elements** can appear more than n/3 times.

This is the first interview checkpoint.

If candidate doesn't derive this, they won’t reach optimal logic.

---

# 🔷 Step 2: Brute Force Thinking (Baseline)

### Approach 1: Frequency Map

```java
Map<Integer, Integer> map = new HashMap<>();

for (int num : nums) {
    map.put(num, map.getOrDefault(num, 0) + 1);
}

List<Integer> ans = new ArrayList<>();

for (int key : map.keySet()) {
    if (map.get(key) > nums.length / 3) {
        ans.add(key);
    }
}
```

### Complexity

* Time: O(n)
* Space: O(n)

### Interview Reaction

Interviewer says:

> Can you do better in space?

Now real thinking starts.

---

# 🔷 Step 3: Elimination Logic (Without Naming Any Algorithm)

Since:

* Only **2 elements max** can satisfy the condition
* We don’t need to track all frequencies
* We just need to track **2 potential winners**

So instead of storing all elements,
we maintain:

```
candidate1
candidate2
count1
count2
```

---

# 🔷 Step 4: Why Elimination Works?

Think of this as a **voting cancellation process**.

If you see a new number and both candidate slots are filled,
you reduce both counts.

Why?

Because:

* That new number can “cancel” one vote from each current candidate.
* If an element truly appears > n/3 times,
  it can survive all cancellations.

This is the interview-level reasoning:

> Elements with low frequency will get eliminated.
> Only heavy-frequency elements survive elimination waves.

---

# 🔷 Step 5: Build Logic Step-by-Step

For every number:

### Case 1:

If number matches candidate1 → increment count1

### Case 2:

Else if number matches candidate2 → increment count2

### Case 3:

Else if count1 == 0 → assign new candidate1

### Case 4:

Else if count2 == 0 → assign new candidate2

### Case 5:

Else → decrement both counts

That’s the elimination phase.

---

# 🔷 Step 6: Why Second Pass is Required?

Because after elimination:

Candidates are **potential winners**,
not guaranteed winners.

Example:

```
[1,2,3,4]
```

After elimination you might end up with something,
but none actually appear > n/3.

So we re-count.

---

# 🔷 Final Optimized Code (Java)

```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {

        int candidate1 = 0, candidate2 = 0;
        int count1 = 0, count2 = 0;

        for (int num : nums) {

            if (num == candidate1) {
                count1++;
            }
            else if (num == candidate2) {
                count2++;
            }
            else if (count1 == 0) {
                candidate1 = num;
                count1 = 1;
            }
            else if (count2 == 0) {
                candidate2 = num;
                count2 = 1;
            }
            else {
                count1--;
                count2--;
            }
        }

        // Verification pass
        count1 = 0;
        count2 = 0;

        for (int num : nums) {
            if (num == candidate1) count1++;
            else if (num == candidate2) count2++;
        }

        List<Integer> result = new ArrayList<>();

        if (count1 > nums.length / 3) result.add(candidate1);
        if (count2 > nums.length / 3) result.add(candidate2);

        return result;
    }
}
```

---

# 🔷 Time & Space Complexity

| Metric | Value |
| ------ | ----- |
| Time   | O(n)  |
| Space  | O(1)  |

---

# 🔥 Interview Level Explanation (How You Should Speak)

If interviewer asks:

> Why does this work?

You answer:

1. At most 2 elements can exceed n/3.
2. We maintain 2 slots.
3. We eliminate 3 different elements together.
4. True majority elements cannot be fully cancelled.
5. Final pass confirms validity.

That’s clean, confident reasoning.

---


* 🔥 Convert this into handwritten-style revision notes (.md)

Your call 🚀
