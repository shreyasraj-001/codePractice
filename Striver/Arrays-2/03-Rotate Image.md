# 📌 48. Rotate Image

🔗 **Problem Link:**
[https://leetcode.com/problems/rotate-image/](https://leetcode.com/problems/rotate-image/)

---

## 1️⃣ Problem Statement

Given an **n × n 2D matrix**, rotate the matrix **90 degrees clockwise**, **in-place**.

* The matrix must be modified directly
* No extra 2D matrix allowed

---

## 2️⃣ Example

### Input

```
[
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]
```

### Output

```
[
  [7, 4, 1],
  [8, 5, 2],
  [9, 6, 3]
]
```

---

## 3️⃣ Core Insight

A **90° clockwise rotation** can be achieved using **two deterministic steps**:

1. **Transpose the matrix**
2. **Reverse each row**

This guarantees:

* In-place modification
* Optimal time & space

---

## 4️⃣ Step-by-Step Logic

### ✅ Step 1: Transpose the Matrix

Swap elements across the main diagonal:

```
matrix[i][j] ↔ matrix[j][i]
```

Example:

```
1 2 3        1 4 7
4 5 6  --->  2 5 8
7 8 9        3 6 9
```

---

### ✅ Step 2: Reverse Each Row

Reverse elements of every row:

```
1 4 7        7 4 1
2 5 8  --->  8 5 2
3 6 9        9 6 3
```

---

## 5️⃣ Java Implementation (Optimized)

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;

        // Step 1: Transpose
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

        // Step 2: Reverse each row
        for (int i = 0; i < n; i++) {
            int left = 0, right = n - 1;
            while (left < right) {
                int temp = matrix[i][left];
                matrix[i][left] = matrix[i][right];
                matrix[i][right] = temp;
                left++;
                right--;
            }
        }
    }
}
```

---

## 6️⃣ Time & Space Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | **O(n²)**  |
| Space  | **O(1)**   |

✔ Every element must be visited
✔ No extra memory used

---

## 7️⃣ Similar Problems (Same Pattern 🔁)

## 🧩 Core Pattern Definition

Problems where you:

* Rearrange elements using **index mapping**
* Use **transpose**, **row/column reversal**
* Rotate, flip, or mirror a matrix
* Often require **in-place modification**

---

## 🔹 1. Rotation-Based Problems (Closest Match)

These are **direct siblings** of Rotate Image.

1. **Rotate Image (90° Clockwise)** – LeetCode 48
2. **Rotate Image (90° Anti-Clockwise)**

   * Transpose + reverse columns
3. **Rotate Image (180°)**

   * Reverse rows + reverse columns
4. **Rotate Image (270°)**

   * Transpose + reverse columns
5. **Rotate Matrix by K times**
6. **Rotate Square Matrix In-Place**

📌 Pattern: **Index transformation + symmetry**

---

## 🔹 2. Transpose-Centric Problems

Problems where **row ↔ column** logic is key.

7. **Transpose Matrix** – LeetCode 867
8. **Matrix Transpose In-Place (Square Matrix)**
9. **Convert Rows to Columns**
10. **Main Diagonal / Secondary Diagonal Traversal**
11. **Check if Matrix is Symmetric**

📌 Pattern: **matrix[i][j] ↔ matrix[j][i]**

---

## 🔹 3. Row / Column Reversal Problems

Problems that use **reverse logic** similar to step-2 of Rotate Image.

12. **Reverse Rows of a Matrix**
13. **Reverse Columns of a Matrix**
14. **Flipping an Image** – LeetCode 832
15. **Mirror Image of Matrix**
16. **Horizontal Flip of Matrix**
17. **Vertical Flip of Matrix**

📌 Pattern: **Two-pointer reversal**

---

## 🔹 4. Index Mapping / Coordinate Transformation

Problems where each element moves to a **new computed position**.

18. **Reshape the Matrix** – LeetCode 566
19. **Matrix Rotation with Extra Space**
20. **Build Matrix from Rotation Rules**
21. **Convert 1D → 2D Matrix** – LeetCode 2022
22. **Diagonal Traverse** – LeetCode 498

📌 Pattern: **Old index → new index**

---

## 🔹 5. Matrix Traversal Patterns (Related Thinking)

Not rotation, but same **spatial reasoning** skill.

23. **Spiral Matrix** – LeetCode 54
24. **Spiral Matrix II** – LeetCode 59
25. **Print Matrix in Spiral Order**
26. **Wave Traversal of Matrix**
27. **Zig-Zag Matrix Traversal**

📌 Pattern: **Directional movement & boundaries**

---

## 🔹 6. Matrix Modification In-Place (Interview Gold)

These test **in-place discipline**, like Rotate Image.

28. **Set Matrix Zeroes** – LeetCode 73
29. **Game of Life** – LeetCode 289
30. **Replace Elements with Greatest on Right**
31. **Modify Matrix Based on Conditions**
32. **Boolean Matrix Problem**

📌 Pattern: **Careful overwriting + constraints**

---

## 🔹 7. Competitive Programming / Advanced Variants

33. **Rotate Matrix Ring by Ring**
34. **Rotate Only Outer Layer of Matrix**
35. **Rotate Matrix Anti-Diagonally**
36. **Rotate Non-Square Matrix (with space)**
37. **Rotate Submatrix Queries**



