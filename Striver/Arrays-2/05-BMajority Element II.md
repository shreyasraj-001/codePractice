## 🔢 [Majority Element II](https://leetcode.com/problems/majority-element-ii/)

**Problem (LeetCode 229)**
Given an integer array `nums`, return all elements that appear **more than ⌊n/3⌋ times**.

👉 There can be **at most 2 majority elements**.

---

## 🧠 Core Idea (Extended Moore’s Voting Algorithm)

In the normal Majority Element problem (> n/2), we keep **1 candidate**.

Here, threshold is **n/3**, so:

* Maximum possible majority elements = **2**
* So we maintain:

  * `candidate1`, `candidate2`
  * `count1`, `count2`

---

## 💡 Why Maximum 2?

If three numbers each appear more than n/3 times:

```
3 × (n/3) = n
```

No space left for others → impossible.

So only **2 elements** can satisfy > n/3.

---

# 🧩 Algorithm Steps

### 🔹 Step 1: Find Potential Candidates

Loop through array:

```
If num == candidate1 → count1++
Else if num == candidate2 → count2++
Else if count1 == 0 → candidate1 = num, count1 = 1
Else if count2 == 0 → candidate2 = num, count2 = 1
Else → count1-- and count2--
```

👉 This is **pair cancellation logic**.

---

### 🔹 Step 2: Verify Candidates

Because Moore's algorithm only gives potential candidates.

Count their actual frequency and check:

```
if freq > n/3 → add to answer
```

---

# 🖥 Java Implementation (Preferred for DSA)

```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {

        int count1 = 0, count2 = 0;
        Integer candidate1 = null, candidate2 = null;

        // Phase 1: Find candidates
        for (int num : nums) {

            if (candidate1 != null && num == candidate1) {
                count1++;
            }
            else if (candidate2 != null && num == candidate2) {
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

        // Phase 2: Verify
        count1 = 0;
        count2 = 0;

        for (int num : nums) {
            if (num == candidate1) count1++;
            else if (num == candidate2) count2++;
        }

        List<Integer> result = new ArrayList<>();
        int n = nums.length;

        if (count1 > n / 3) result.add(candidate1);
        if (count2 > n / 3) result.add(candidate2);

        return result;
    }
}
```

---

# 📊 Example Visualization

Input:

```
[1,2,3,1,2,1,1]
n = 7
n/3 = 2
```

Final frequency:

```
1 → 4 times ✔
2 → 2 times ✘
3 → 1 time ✘
```

Output:

```
[1]
```

---

# ⏱ Complexity

* Time: **O(n)**
* Space: **O(1)**

---

# 🔥 Important Interview Insights

* Always remember:

  * For n/2 → 1 candidate
  * For n/3 → 2 candidates
  * For n/k → k-1 candidates

* Order of conditions is VERY important.

---

If you want, I can now:

* 🔍 Explain why order matters (very important trap)
* 📈 Do full dry run on tricky input
* 📝 Generate handwritten-style revision notes in `.md`
* 🔁 Connect it with "More than n/k Occurrences" pattern
