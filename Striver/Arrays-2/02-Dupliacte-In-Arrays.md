# 📌 Array Duplicates — [ **GeeksforGeeks – Find Duplicates in an Array**](https://www.geeksforgeeks.org/problems/find-duplicates-in-an-array/1)
---

## 1️⃣ Explain the Problem (with link)

### 🔹 Problem Statement

You are given an integer array `arr` of size `n`.

* All elements are in the range **1 to n**
* Each element appears **at most twice**
* Your task is to return **all elements that appear exactly twice**
* The result can be returned in any order (driver code will sort it)

### 🔹 Problem Link


### 🔹 Examples

**Input**

```
arr = [1, 2, 3, 2, 3]
```

**Output**

```
[2, 3]
```

**Explanation**
2 and 3 occur more than once.

---

**Input**

```
arr = [1, 2, 3, 4]
```

**Output**

```
[]
```

**Explanation**
No element repeats.

---

## 2️⃣ Similar Problems (Same Pattern)

* Find All Numbers Disappeared in an Array
* First Missing Positive
* Set Mismatch
* Find the Duplicate Number
* Missing and Repeating Numbers

👉 **Common Pattern:**

> Index mapping using given constraints (1 to n)

---

## 3️⃣ Core Logic Behind the Problem & Main Challenge

### 🔹 Core Constraint Insight

* Values are limited to **1 to n**
* Indices also range from **0 to n-1**

This allows us to **map values to indices**.

### 🔹 Main Challenge

* Must avoid extra space
* Must efficiently detect elements appearing **twice**
* Need to respect time constraints

---

## 4️⃣ Brute Force Solution & Why It’s Not Acceptable

### 🔹 Brute Force Idea

* For every element, count its frequency using nested loops

  ### 🔹 Code

```java
import java.util.*;

class Solution {
    public ArrayList<Integer> findDuplicatesBruteForce(int[] arr) {
        ArrayList<Integer> result = new ArrayList<>();
        int n = arr.length;

        for (int i = 0; i < n; i++) {
            int count = 0;

            for (int j = 0; j < n; j++) {
                if (arr[i] == arr[j]) {
                    count++;
                }
            }

            if (count == 2 && !result.contains(arr[i])) {
                result.add(arr[i]);
            }
        }
        return result;
    }
}
```

### ❌ Why It’s Not Acceptable

* Time Complexity: **O(n²)**
* Too slow for large inputs
* Repeated comparisons


### 🔹 Time Complexity

```
O(n²)
```

### 🔹 Why It’s Not Acceptable

* Inefficient for large inputs
* Fails time limits
* Not interview-safe

---

## 5️⃣ All Methods & Logic Behind Each Method

---

### 🔹 Method 1: Using Extra Array / List (Rejected)

### 🔹 Logic

* Create a frequency array of size `n + 1`
* Count occurrences
* Add elements with count = 2

### 🔹 Code

```java
import java.util.*;

class Solution {
    public ArrayList<Integer> findDuplicatesUsingExtraArray(int[] arr) {
        ArrayList<Integer> result = new ArrayList<>();
        int n = arr.length;

        int[] freq = new int[n + 1];

        for (int num : arr) {
            freq[num]++;
        }

        for (int i = 1; i <= n; i++) {
            if (freq[i] == 2) {
                result.add(i);
            }
        }
        return result;
    }
}
```

## 🔴 The Confusing Line (Focus Area)

```java
for (int num : arr) {
    freq[num]++;
}
```

## 📌 Example 1 (With Duplicates)

### Input

```java
arr = [1, 1, 3, 2, 3]
n = 5
```

### Step 1: Create `freq` array

```java
int[] freq = new int[n + 1];
```

So initially:

| Index (value) | 0 | 1 | 2 | 3 | 4 | 5 |
| ------------- | - | - | - | - | - | - |
| freq[]        | 0 | 0 | 0 | 0 | 0 | 0 |

> We ignore index `0`, we use `1 to n`

---

## 🔁 Step 2: Loop Visualization

### Loop Code

```java
for (int num : arr) {
    freq[num]++;
}
```

---

### 🔄 Iteration 1

```java
num = 1
freq[1]++
```

| Index | 1 |
| ----- | - |
| freq  | 1 |

Full array now:

```
freq = [0, 1, 0, 0, 0, 0]
```

### 🔄 Iteration 2

```java
num = 1
freq[1]++
```

```
freq = [0, 2, 0, 0, 0, 0]
```
### 🔄 Iteration 3

```java
num = 3
freq[3]++
```

```
freq = [0, 2, 0, 1, 0, 0]
```
## 🧠 Mental Model (Remember This Always)

> **“Array value decides which index to update”**
```java
freq[arr[i]]++;
```



---

### ❌ Why Rejected

* Extra space used
* Problem hints allow **in-place solution**

**Logic**

* Maintain a frequency array or list
* Count occurrences
* Pick elements with count = 2

**Issues**

* Uses extra space
* Breaks space-optimized requirement

❌ Not preferred in interviews

---

### 🔹 Method 2: HashMap Frequency Count

### 🔹 Logic

* Store frequency using HashMap
* Collect elements with frequency = 2

### 🔹 Code

```java
import java.util.*;

class Solution {
    public ArrayList<Integer> findDuplicatesUsingHashMap(int[] arr) {
        ArrayList<Integer> result = new ArrayList<>();
        HashMap<Integer, Integer> map = new HashMap<>();

        for (int num : arr) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }

        for (int key : map.keySet()) {
            if (map.get(key) == 2) {
                result.add(key);
            }
        }
        return result;
    }
}
```

### ⚠️ Drawback

* Uses extra space
* Interviewers prefer **O(1) space** when possible

**Time:** `O(n)`
**Space:** `O(n)`

⚠️ Works, but **not optimal**

---

### 🔹 Method 3:  Index Marking (Optimal Method ✅)
### 🔹 Logic
  
1. Traverse the array
2. Convert value to index: `idx = abs(arr[i]) - 1`
3. If value at index is negative → duplicate found
4. Else mark it negative

✔ Uses input array as frequency map
✔ No extra space
✔ Fast and clean

### 🔹 Code (Best Solution)

```java
import java.util.*;

class Solution {
    public ArrayList<Integer> findDuplicates(int[] arr) {
        ArrayList<Integer> result = new ArrayList<>();

        for (int i = 0; i < arr.length; i++) {
            int index = Math.abs(arr[i]) - 1;

            if (arr[index] < 0) {
                result.add(index + 1);
            } else {
                arr[index] = -arr[index];
            }
        }
        return result;
    }
}
```

### ✅ Why This Is Best

* Time: **O(n)**
* Space: **O(1)** (excluding output)
* Uses given constraints smartly
* Interview favorite

---

## 📊 Quick Comparison

| Method        | Time  | Space | Interview Safe |
| ------------- | ----- | ----- | -------------- |
| Brute Force   | O(n²) | O(1)  | ❌              |
| Extra Array   | O(n)  | O(n)  | ❌              |
| HashMap       | O(n)  | O(n)  | ⚠️             |
| Index Marking | O(n)  | O(1)  | ✅              |


## 6️⃣ Why the Best Method Works

### 🔹 Key Reason

Because:

* Each number belongs to range `1 to n`
* Each value maps uniquely to an index

### 🔹 Mathematical Mapping

```
value x → index (x - 1)
```

Negative marking ensures:

* First visit → mark
* Second visit → duplicate detected

---

## 7️⃣ Comparison Summary

| Method        | Extra Space | Time  | Interview Safe |
| ------------- | ----------- | ----- | -------------- |
| Brute Force   | ❌           | O(n²) | ❌              |
| HashMap       | ✅           | O(n)  | ⚠️             |
| Index Marking | ❌           | O(n)  | ✅              |

---

## 8️⃣ Time & Space Complexity

### 🔹 Optimal Method

* **Time:** `O(n)`
* **Space:** `O(1)` (ignoring output list)

---

## 9️⃣ Key Takeaways (Must Remember)

✔ Use given constraints cleverly
✔ Value range `1 to n` hints at **index mapping**
✔ Negative marking is a powerful array trick
✔ Avoid HashMap when in-place solution exists
✔ Frequently asked in interviews

---

### 🔥 One-Line Interview Summary

> “Use the array itself as a frequency map by marking visited indices negative.”

---

If you want next:

* Dry run with large input
* Java code walkthrough line-by-line
* Convert this into **quick revision notes**
* Compare with Merge Intervals pattern

Just say 👍
