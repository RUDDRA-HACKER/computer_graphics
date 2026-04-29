## Circle Generation (Midpoint Circle Algorithm)
The Midpoint Circle Algorithm is an efficient method for rasterizing circles. It uses integer arithmetic to determine the points that best approximate a circle on a pixel grid. The algorithm starts at the top of the circle and iteratively determines the next point based on a decision parameter that evaluates the midpoint between two potential pixel choices.

# Circle Generation (Midpoint Circle Algorithm)

## Definition

An efficient algorithm to draw a circle using integer calculations and symmetry.

---

## Circle Equation

[
x^2 + y^2 = r^2
]

---

## Basic Idea

Instead of calculating all points, we calculate only one part and use symmetry.

---

## Symmetry (Very Important)

Circle has 8-way symmetry:

* ( x,  y)
* (-x,  y)
* ( x, -y)
* (-x, -y)
* ( y,  x)
* (-y,  x)
* ( y, -x)
* (-y, -x)

Only 1/8 part is calculated, rest are mirrored.

---

## Initial Decision Parameter

$$
p_0 = 1 - r
$$

---

## Algorithm Steps

1. Input radius `r`
2. Start point:

   * x = 0
   * y = r
3. Calculate:

   * p = 1 - r
4. Plot initial 8 points
5. Repeat while `x < y`

---

## Iteration Rules

### Case 1: If p < 0

Choose East (E)

* x = x + 1
* p = p + 2x + 3

---

### Case 2: If p ≥ 0

Choose South-East (SE)

* x = x + 1
* y = y - 1
* p = p + 2x - 2y + 5

---

## Example (r = 5)

| Step | x | y | p  | Action |
| ---- | - | - | -- | ------ |
| 0    | 0 | 5 | -4 | E      |
| 1    | 1 | 5 | -1 | E      |
| 2    | 2 | 5 | 4  | SE     |
| 3    | 3 | 4 | 3  | SE     |
| 4    | 4 | 3 | —  | Stop   |

---

## Final Points (1/8 Circle)

(0,5), (1,5), (2,5), (3,4), (4,3)

Apply symmetry to get full circle.

---

## Advantages

* Fast (uses integer calculations)
* No floating-point operations
* Efficient
* Widely used

---

## Disadvantages

* Slight approximation
* Harder to understand initially

---

## Simple Understanding

* p < 0 → move straight (E)
* p ≥ 0 → move diagonally (SE)

---

## Summary

Midpoint Circle Algorithm draws a circle using a decision parameter and 8-way symmetry.

# Ellipse Generation (Midpoint Ellipse Algorithm)

## Definition

An efficient algorithm to draw an ellipse using integer calculations and symmetry.

---

## Ellipse Equation
$F(x,y)=\frac{x^2}{a^2}+\frac{y^2}{b^2}-1$

## Basic Idea

Instead of computing all points, we calculate points in one quadrant and use symmetry.

---

## Symmetry (Very Important)

Ellipse has 4-way symmetry:

* ( x,  y)
* (-x,  y)
* ( x, -y)
* (-x, -y)

---

## Regions of Ellipse

Ellipse is divided into **2 regions**:

* **Region 1:** slope < 1
* **Region 2:** slope ≥ 1

---

## Initial Values

* Start point:

  * x = 0
  * y = b

---

## Region 1 (slope < 1)

### Initial Decision Parameter


$$
p_1 = b^2 - a^2b + \frac{1}{4}a^2
$$


---

### Iteration Rules

#### If p₁ < 0

Choose East (E)

* x = x + 1
* p₁ = p₁ + 2b²x + b²

#### If p₁ ≥ 0

Choose South-East (SE)

* x = x + 1
* y = y - 1
* p₁ = p₁ + 2b²x - 2a²y + b²

---

## Region 2 (slope ≥ 1)

### Initial Decision Parameter

$$
p_2 = b^2(x + \frac{1}{2})^2 + a^2(y - 1)^2 - a^2b^2
$$

---

### Iteration Rules

#### If p₂ > 0

Choose South (S)

* y = y - 1
* p₂ = p₂ - 2a²y + a²

#### If p₂ ≤ 0

Choose South-East (SE)

* x = x + 1
* y = y - 1
* p₂ = p₂ + 2b²x - 2a²y + a²

---

## Algorithm Steps

1. Input a (major axis) and b (minor axis)
2. Start at (0, b)
3. Calculate p₁
4. Process Region 1 until slope = 1
5. Calculate p₂
6. Process Region 2 until y = 0
7. Plot symmetric points

---

## Example (a = 5, b = 3)

Initial:

* (x, y) = (0, 3)

Points generated (one quadrant):

* (0,3), (1,3), (2,2), (3,1)

Apply symmetry to get full ellipse.

---

## Advantages

* Uses integer calculations
* More efficient than direct equation
* Accurate plotting

---

## Disadvantages

* More complex than circle algorithm
* Requires handling two regions

---

## Simple Understanding

* Region 1 → move more in x direction
* Region 2 → move more in y direction

---



## **boundary fill algorithm**:
1. Start from a seed point inside the area to be filled.
2. Check the color of the current pixel:
   - If it is not the boundary color and not the fill color, change it to the fill color.
   - Recursively apply the same process to the neighboring pixels (up, down, left, right).  


![Boundary_Fill_Algorithm](/image/Boundry-1.png)

Image 1 (Boundary-1.png):
This shows the starting point of the boundary fill algorithm. There's a closed shape (boundary) drawn on the screen, and a seed point is selected inside the area that needs to be filled.
![Boundary_Fill_Algorithm](/image/Boundry-2.png)
Image 2 (Boundary-2.png):
The algorithm begins filling from the seed point. It checks neighboring pixels and fills those that are not the boundary color. The fill starts expanding outward in the adjacent areas.

![Boundary_Fill_Algorithm](/image/Boundry-3.png)

Image 3 (Boundary-3.png):
The filling continues to spread to more neighboring pixels. The algorithm recursively fills all connected pixels that meet the fill criteria (not boundary color, not already filled).
![Boundary_Fill_Algorithm](/image/Boundry-4.png)
Image 4 (Boundary-4.png):
The filling is complete. The entire area inside the boundary has been filled with the fill color. The algorithm stops when it encounters the boundary, ensuring the fill stays within the defined shape.

4-way connectivity (4-connected):

Checks 4 neighboring pixels: Up, Down, Left, Right
Used by the Boundary Fill Algorithm
Fills pixels that are directly adjacent (not diagonal)
8-way connectivity (8-connected):

Checks 8 neighboring pixels: Up, Down, Left, Right, and 4 Diagonals
Used by the Flood Fill Algorithm
Fills pixels that are adjacent AND diagonal neighbors
Visual representation:
 
      4-way:             8-way:
        ↑                ↖ ↑ ↗
      ← • →              ← • →
        ↓                ↙ ↓ ↘
## **flood fill algorithm**:
1. Start from a seed point inside the area to be filled.
2. Check the color of the current pixel:
   - If it is not the boundary color and not the fill color, change it to the fill color.
   - Recursively apply the same process to all neighboring pixels (including diagonals).
## Why Use Flood Fill Algorithm Instead of Boundary Fill Algorithm?

*When in a boundary fill with multiple colour then it will not work properly because it will stop filling when it encounters a different colour which is not the boundary colour. In contrast, the flood fill algorithm will continue to fill all connected pixels regardless of their color, as long as they are not the boundary color. This makes the flood fill algorithm more robust and effective in scenarios where there are multiple colors within the area to be filled.*

![Flood_Fill_Algorithm](/image/flood_fill.png)

I will map the whiteboard exactly into simple step-by-step algorithm steps for both 4-connected and 8-connected flood fill, including the order shown in the photo.According to the photo, these are the complete steps of Flood Fill.

1. Choose a seed pixel inside the region, for example $(x,y)$.
2. Read the old color at that pixel, call it $o$.
3. Choose the new fill color, call it $n$.
4. Check condition: if current pixel color is not $o$, stop for that pixel.
5. If current pixel color is $o$, paint it with $n$.
6. Call flood fill on neighbors.

For 4-connected fill in the same order shown:

1. `flood(x+1, y, n, o)` right
2. `flood(x, y+1, n, o)` down
3. `flood(x-1, y, n, o)` left
4. `flood(x, y-1, n, o)` up

For 8-connected fill, do the above 4 first, then add diagonals:

1. `flood(x-1, y-1, n, o)` top-left
2. `flood(x-1, y+1, n, o)` bottom-left
3. `flood(x+1, y-1, n, o)` top-right
4. `flood(x+1, y+1, n, o)` bottom-right

What the grid in the photo means:

1. `R` cells are boundary or blocked cells.
2. `G` cells are filled cells.
3. Fill starts from the seed and spreads to all allowed neighbors.
4. Fill stops automatically when boundary cells are reached.

Compact pseudocode matching the board:

```text

flood(x, y, n, o):
    if getpixel(x, y) == o:
        putpixel(x, y, n)

        // 4-connected
        flood(x+1, y, n, o)
        flood(x, y+1, n, o)
        flood(x-1, y, n, o)
        flood(x, y-1, n, o)

        // extra for 8-connected
        flood(x-1, y-1, n, o)
        flood(x-1, y+1, n, o)
        flood(x+1, y-1, n, o)
        flood(x+1, y+1, n, o)

```

Optimizing tool selection...## Attributes of Graphics Primitives

### 1. **Point Attributes**

A point is the simplest graphics primitive. Its main attributes are:

- **Position:** Coordinates (x, y) that define where the point is located
- **Color:** The color displayed at that point (RGB values)
- **Size:** The diameter or radius of the point (how visible it is)
- **Intensity:** The brightness level of the point

### 2. **Line Attributes**

Lines connect two points. Their attributes include:

- **Endpoints:** Starting point (x₁, y₁) and ending point (x₂, y₂)
- **Color:** The color of the line
- **Width/Thickness:** The thickness of the line (in pixels)
- **Line Style:** The pattern of the line
  - Solid line (————)
  - Dashed line (- - - -)
  - Dotted line (. . . .)
  - Dash-dot line (- . - .)
- **Line Cap:** How line endpoints are drawn
  - Butt cap (flat end)
  - Round cap (rounded end)
  - Projecting cap (extended end)
- **Line Join:** How two lines meet at a corner
  - Miter join (sharp corner)
  - Bevel join (beveled corner)
  - Round join (rounded corner)
- **Intensity/Brightness:** The brightness level

### 3. **Curve Attributes**

Curves are more complex primitives. Their attributes include:

- **Type:** Circle, ellipse, parabola, Bezier curve, spline, etc.
- **Control Points:** Points that define the shape of the curve
- **Color:** The color of the curve
- **Width/Thickness:** The thickness of the curve line
- **Style:** Solid, dashed, dotted, etc.
- **Smoothness:** How smooth the curve appears
- **Fill Color:** If the curve encloses an area
- **Intensity:** Brightness or transparency level

### **Comparison Table**

| Attribute | Point | Line | Curve |
|-----------|-------|------|-------|
| Position | Single (x,y) | Two endpoints | Multiple control points |
| Color | Yes | Yes | Yes |
| Size/Width | Point size | Line width | Curve width |
| Style | — | Solid, dashed, dotted | Solid, dashed, dotted |
| Fill | No | No | Yes (if closed) |

### **Summary**

All graphics primitives share common attributes like **color** and **intensity**, but each has unique attributes based on its complexity. Points are simplest, lines have width and style, and curves have additional properties like control points and smoothness.