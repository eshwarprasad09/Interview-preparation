````markdown
# 🔄 Rotate Image (90° Clockwise) - In-place Matrix Rotation

## 🧠 Problem Intuition

You're given an `n x n` 2D matrix representing an image. Your task is to rotate it 90 degrees clockwise **in-place**, meaning you can't use any extra space or new matrix.

### 🧩 Real-Life Analogy

Think of rotating a photo in a gallery app. You're not changing the content, just its orientation. Behind the scenes, the data structure representing that image (a 2D matrix) is being transformed.

---

## ✅ Algorithm Used: Transpose + Reverse

### 📌 Step 1: **Transpose the Matrix**
Swap elements at positions `[i][j]` with `[j][i]` to convert rows into columns.

### 📌 Step 2: **Reverse Each Row**
After transposing, reverse the elements of each row to complete the 90° clockwise rotation.

---

## 🔍 How to Identify the Pattern

- Recognize that rotating a square matrix 90° clockwise is a well-known transformation.
- The formula for 90° rotation is:  
  `matrix[i][j]` becomes `matrix[j][n - 1 - i]`
- To avoid using extra space, decompose the transformation into two simpler in-place operations:
  1. **Transpose** the matrix.
  2. **Reverse** each row.

---

## 💻 Step-by-Step Code Explanation

```java
public void rotate(int[][] matrix) {
    // Step 1: Transpose the matrix (mirror along the diagonal)
    for(int i = 0; i < matrix.length; i++){
        for(int j = i; j < matrix[0].length; j++){
            // Swap matrix[i][j] and matrix[j][i]
            int temp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = temp;
        }
    }

    // Step 2: Reverse each row to complete the rotation
    for(int i = 0; i < matrix.length; i++){
        int p1 = 0;
        int p2 = matrix[0].length - 1;
        while(p1 < p2){
            int temp = matrix[i][p1];
            matrix[i][p1] = matrix[i][p2];
            matrix[i][p2] = temp;
            p1++;
            p2--;
        }
    }
}
````

---

## 🔄 Step-by-Step Dry Run

### Input:

```java
matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]
```

### ✅ Step 1: Transpose

Swap elements `[i][j]` with `[j][i]` (only for `j ≥ i`)

```java
[
  [1, 4, 7],
  [2, 5, 8],
  [3, 6, 9]
]
```

### ✅ Step 2: Reverse each row

```java
[
  [7, 4, 1],
  [8, 5, 2],
  [9, 6, 3]
]
```

🎉 This is the final rotated matrix.

---

## 🧠 Concepts & Patterns Used

* Matrix Transposition
* In-place Reversal
* 2D Array Index Manipulation
* Space Optimization Techniques

---

## 🧩 How to Break Down This Problem

1. Recognize that rotation ≠ swapping elements in one go.
2. Decompose into two operations that are easy to perform in-place.
3. Realize the advantage of transpose + reverse strategy when **no extra space** is allowed.

---

## 🔁 Summary for Quick Revision

* 🔁 **Transpose**: `matrix[i][j] ⇄ matrix[j][i]`
* 🔁 **Reverse each row** to achieve 90° clockwise
* 🧠 In-place → Space-efficient (O(1))
* 📊 Time Complexity: O(n²)
* 📦 Space Complexity: O(1)
* 💡 Common in image processing, gaming grids, or matrix-heavy computations

---

## ⏱ Time and Space Complexity

| Metric           | Value         |
| ---------------- | ------------- |
| Time Complexity  | O(n²)         |
| Space Complexity | O(1) in-place |

---

## 📌 Real-World Use Case

* **Image rotation** in applications like Google Photos, Adobe Photoshop
* **Game boards** rotation in chess, sudoku, or puzzle apps
* **Graphics libraries** that deal with pixel manipulation

---

## 📚 Additional Learning

* 2D array traversal patterns
* In-place operations vs using auxiliary space
* Variants: Rotate by 180°, 270°, or counter-clockwise (transpose + column reverse)

---

## 🧪 Edge Case to Test

```java
matrix = [
  [1]
]
```

Should return:

```java
[
  [1]
]
```

---

Let me know if you want this version adapted for:

* 90° counter-clockwise rotation
* Non-square (m x n) matrix (though not possible in-place for this problem)
* Visual diagram or animation

```

Let me know if you'd like this explanation exported as a PDF or displayed in a collapsible UI format.
```
