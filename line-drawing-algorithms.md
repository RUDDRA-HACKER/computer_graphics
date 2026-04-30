## point and line 
1. **point** are fondamental elements in computer graphics, representing a specific location in a two-dimensional or three-dimensional space. A point is typically defined by its coordinates, such as (x, y) in 2D space or (x, y, z) in 3D space. Points are used to create more complex graphical elements, such as lines, curves, and shapes.

2. **line** is a fundamental graphical element that represents a straight path between two points in a two-dimensional or three-dimensional space. A line is defined by its endpoints, which are represented as points with specific coordinates. In computer graphics, lines are used to create various shapes, such as polygons and curves, and are essential for rendering images and visualizations. Line drawing algorithms are used to determine which pixels should be illuminated to represent a line on a raster display, ensuring that the line appears smooth and accurate.

### Line Drawing Algorithms
Line drawing algorithms are techniques used in computer graphics to determine which pixels should be illuminated to represent a line on a raster display. These algorithms are designed to create smooth and accurate lines while minimizing the computational resources required. Some of the most commonly used line drawing algorithms include:
1. **DDA (Digital Differential Analyzer) Algorithm**: This algorithm uses floating-point arithmetic to calculate the intermediate points along the line. It incrementally calculates the next point based on the previous point and the slope of the line. While it is simple to implement, it can be inefficient due to the use of floating-point calculations.

🧮 Steps

Given:
(x1, y1) and (x2, y2)
1. Calculate the differences in x and y coordinates:
   dx = x2 - x1
   dy = y2 - y1 
2. Determine the number of steps required to draw the line:
   steps = max(abs(dx), abs(dy))    
   abs means the absolute value function, which returns the non-negative value of its argument. In this context, it is used to ensure that the number of steps is determined based on the larger of the two differences (dx and dy), regardless of their signs.
3. Calculate the increment in x and y coordinates for each step:
  - x_increment = dx / steps 
  -  y_increment = dy / steps
4. Initialize the starting point:
    - x = x1
    - y = y1
5. For each step from 0 to steps, do the following:
   - Plot the point at (round(x), round(y))
    - Increment x and y by their respective increments:
      - x += x_increment
      - y += y_increment
      example:
      Given two points (2, 3) and (10, 7):  
1. Calculate the differences:
   - dx = 10 - 2 = 8
   - dy = 7 - 3 = 4
2. Determine the number of steps:
   - steps = max(abs(8), abs(4)) = 8
3. Calculate the increments:
   - x_increment = 8 / 8 = 1
   - y_increment = 4 / 8 = 0.5
4. Initialize the starting point:
   - x = 2
   - y = 3
5. For each step from 0 to 8, plot the points:
   - Step 0: Plot (round(2), round(3)) = (2, 3)
   - Step 1: x = 2 + 1 = 3, y = 3 + 0.5 = 3.5 → Plot (round(3), round(3.5)) = (3, 4)
   - Step 2: x = 3 + 1 = 4, y = 3.5 + 0.5 = 4 → Plot (round(4), round(4)) = (4, 4)
   up to Step 8: Plot (round(10), round(7)) = (10, 7)   

 **Bresenham's Line Algorithm**: This algorithm uses integer arithmetic to determine which pixels to illuminate, making it more efficient than the DDA algorithm. It calculates the error term to decide when to increment the y-coordinate while iterating through the x-coordinates.

🧮 Example (for slope 0 < m < 1)

Given:
(x1, y1) = (2, 3) and (x2, y2) = (10, 7)

1. Calculate:
   - dx = x2 - x1 = 10 - 2 = 8
   - dy = y2 - y1 = 7 - 3 = 4

2. Initial decision parameter:
   - p0 = 2dy - dx = 2(4) - 8 = 0

**Note**: *The decision parameter is used to determine the next pixel to plot.*

3. Start from (x, y) = (2, 3). For each step, increment x by 1:
   - If pk < 0: choose E (x+1, y), and pk+1 = pk + 2dy
   - If pk >= 0: choose NE (x+1, y+1), and pk+1 = pk + 2dy - 2dx

4. Iterations:

| Step | pk before choice | Pixel plotted | Choice | pk(next) |
|------|-------------------|---------------|--------|----------|
| 0    | p0 = 0            | (2, 3)        | NE     | -8       |
| 1    | -8                | (3, 4)        | E      | 0        |
| 2    | 0                 | (4, 4)        | NE     | -8       |
| 3    | -8                | (5, 5)        | E      | 0        |
| 4    | 0                 | (6, 5)        | NE     | -8       |
| 5    | -8                | (7, 6)        | E      | 0        |
| 6    | 0                 | (8, 6)        | NE     | -8       |
| 7    | -8                | (9, 7)        | E      | 0        |
| 8    | —                 | (10, 7)       | End    | —        |

Final plotted points:
(2,3), (3,4), (4,4), (5,5), (6,5), (7,6), (8,6), (9,7), (10,7)


pk = decision parameter

Should we go RIGHT (E) or DIAGONAL (NE)?

| pk value | Move          |
| -------- | ------------- |
| pk < 0   | E (right)     |
| pk ≥ 0   | NE (diagonal) |

- Start from (x, y) = (2, 3). For each step, increment x by 1:

  If pk < 0: choose E (x+1, y), and pk+1 = pk + 2dy
  If pk >= 0: choose NE (x+1, y+1), and pk+1 = pk + 2dy - 2dx
pk+1 = pk + 2dy - 2dx
= 0 + 8 - 16 = -8
Step 1:
p = -8 → < 0 → choose E

👉 Move:

(3,4) → (4,4)
🧠 Pattern you saw
0 → -8 → 0 → -8 → ...

👉 That’s why:

NE → E → NE → E → ...

# Mid-Point Line Algorithm (Complete Notes)

---

# 🎯 Definition

The **Mid-Point Line Algorithm** is used to draw a straight line by selecting the pixel closest to the actual line using a decision parameter based on the midpoint between two possible pixels.

---

# 🧠 Key Idea

At each step, we choose between:

* **E (East)** → (x+1, y)
* **NE (North-East)** → (x+1, y+1)

Decision depends on a parameter **d**

---

# 📌 Decision Parameter

[
d = 2dy - dx
]

---

# 🎯 Decision Rule

* If **d < 0** → choose **E**
* If **d ≥ 0** → choose **NE**

---

# 🔁 Update Rules

* If **E chosen**:

  ```text
  d = d + 2dy
  ```

* If **NE chosen**:

  ```text
  d = d + 2dy - 2dx
  ```

---

# 📋 Algorithm Steps (0 < slope < 1)

1. Input points (x1, y1) and (x2, y2)
2. Calculate:

   ```text
   dx = x2 - x1
   dy = y2 - y1
   ```
3. Initialize:

   ```text
   d = 2dy - dx
   x = x1
   y = y1
   ```
4. Plot starting point (x, y)
5. Repeat until x = x2:

   * If d < 0 → choose E
   * If d ≥ 0 → choose NE
6. Update d accordingly
7. Plot each point

---

# 🔢 Example

## Given:

```text
(x1, y1) = (2, 3)
(x2, y2) = (10, 7)
```

---

## Step 1: Calculate

```text
dx = 10 - 2 = 8
dy = 7 - 3 = 4
```

---

## Step 2: Initial value

[
d = 2(4) - 8 = 0
]

---

# 📊 Iteration Table

| Step | d before | Point  | Choice | d after |
| ---- | -------- | ------ | ------ | ------- |
| 0    | 0        | (2,3)  | NE     | -8      |
| 1    | -8       | (3,4)  | E      | 0       |
| 2    | 0        | (4,4)  | NE     | -8      |
| 3    | -8       | (5,5)  | E      | 0       |
| 4    | 0        | (6,5)  | NE     | -8      |
| 5    | -8       | (7,6)  | E      | 0       |
| 6    | 0        | (8,6)  | NE     | -8      |
| 7    | -8       | (9,7)  | E      | 0       |
| End  | —        | (10,7) | —      | —       |

---

# ✅ Final Points

```text
(2,3), (3,4), (4,4), (5,5), (6,5), (7,6), (8,6), (9,7), (10,7)
```

---

# 🧠 Simple Understanding

* d < 0 → move straight (E)
* d ≥ 0 → move diagonal (NE)

---

# 🔥 Important Notes

* Uses integer calculations (fast)
* Avoids floating-point operations
* Same output as Bresenham algorithm
* Widely used in computer graphics

---

# 🎯 Summary

```text
Midpoint algorithm selects the best pixel using a decision parameter to approximate a straight line efficiently.
```

---
