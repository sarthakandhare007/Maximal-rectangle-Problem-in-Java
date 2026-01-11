<img width="594" height="658" alt="image" src="https://github.com/user-attachments/assets/fd795c4a-6860-424d-8c61-0ade87200a09" />



# Maximal-rectangle-Problem-in-Java

This is **LeetCode 85 – Maximal Rectangle**.
I’ll explain it **step-by-step in a simple way** and then give you a **clean Java solution**.

---

## 🔹 Problem in Simple Words

You are given a **binary matrix** (`0` and `1`).

👉 Find the **largest rectangle** that contains **only 1s** and return its **area**.

---

## 🔹 Key Idea (Most Important)

We **convert each row into a histogram** and then solve **Largest Rectangle in Histogram** for every row.

Why?

* A rectangle of 1s can span **multiple rows**
* Each row builds height on top of the previous row

---

## 🔹 Step-by-Step Explanation

### Step 1: Build Heights Array

For each column:

* If matrix[row][col] == `'1'` → `height[col]++`
* If matrix[row][col] == `'0'` → `height[col] = 0`

So every row becomes a **histogram**.

---

### Step 2: Largest Rectangle in Histogram

For each histogram:

* Use a **stack**
* Find max area using:

```
area = height × width
```

This is a **classic problem**.

---

## 🔹 Example Walkthrough

For this matrix:

```
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```

Histograms row by row:

```
[1,0,1,0,0]
[2,0,2,1,1]
[3,1,3,2,2]
[4,0,0,3,0]
```

Maximum rectangle area = **6**

---

## 🔹 Java Solution (Clean & Interview-Ready)

```java
import java.util.Stack;

class Solution {
    public int maximalRectangle(char[][] matrix) {
        if (matrix.length == 0) return 0;

        int cols = matrix[0].length;
        int[] heights = new int[cols];
        int maxArea = 0;

        for (char[] row : matrix) {

            // Build histogram
            for (int j = 0; j < cols; j++) {
                if (row[j] == '1')
                    heights[j]++;
                else
                    heights[j] = 0;
            }

            // Calculate max area in histogram
            maxArea = Math.max(maxArea, largestRectangleArea(heights));
        }

        return maxArea;
    }

    private int largestRectangleArea(int[] heights) {
        Stack<Integer> stack = new Stack<>();
        int maxArea = 0;
        int n = heights.length;

        for (int i = 0; i <= n; i++) {
            int currHeight = (i == n) ? 0 : heights[i];

            while (!stack.isEmpty() && currHeight < heights[stack.peek()]) {
                int height = heights[stack.pop()];
                int width = stack.isEmpty() ? i : i - stack.peek() - 1;
                maxArea = Math.max(maxArea, height * width);
            }
            stack.push(i);
        }

        return maxArea;
    }
}
```

---

## 🔹 Time & Space Complexity

* **Time:** `O(rows × cols)`
* **Space:** `O(cols)`

---
