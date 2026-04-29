# Line Clipping Algorithms - Complete Guide

---

## 🎯 Definition

**Line Clipping** is the process of determining which portions of a line segment lie inside a rectangular clipping window and removing (or not drawing) the parts that fall outside.

---

## 🧠 Why Line Clipping?

### Problems Without Clipping:
```
❌ Draw entire lines even outside viewport
❌ Rendering artifacts at screen edges
❌ Unnecessary processing of invisible geometry
❌ Display errors and flickering
```

### Benefits:
```
✅ Faster rendering (only visible parts)
✅ Clean display without artifacts
✅ Improved performance
✅ Correct viewport handling
```

---

## 📊 Line Clipping Cases

### Case 1: Entire Line Inside Window
```
Window: ┌──────────────┐
        │  ●────●      │
        │ Line inside  │
        └──────────────┘

Action: ACCEPT entire line
Result: Draw line completely
```

### Case 2: Entire Line Outside Window
```
        ●────●
        (outside)

Window: ┌──────────────┐
        │              │
        └──────────────┘

Action: REJECT entire line
Result: Don't draw anything
```

### Case 3: Line Partially Visible
```
●─────┌────●─────┤
       │  Window  │
       └──────────┘

Action: CLIP line
Result: Keep inside portion only
```

---

# 🔪 Cohen-Sutherland Line Clipping Algorithm

## 🎯 Overview

The Cohen-Sutherland algorithm divides the plane around the window into 9 regions using 4-bit outcodes. Each endpoint is assigned an outcode, then the line visibility is determined.

---

## 📍 Outcode Region Classification

### 9-Region Division:

```
        1001  1000  1010
          │     │      │
        ──┼─────┼──────┼──
        0001│ 0000│ 0010│
        ──┼─────┼──────┼──
          │     │      │
        0101  0100  0110
```

### Outcode Bits:

```
Bit 0 (RIGHT):  1 if x > xmax, 0 otherwise
Bit 1 (LEFT):   1 if x < xmin, 0 otherwise
Bit 2 (TOP):    1 if y > ymax, 0 otherwise
Bit 3 (BOTTOM): 1 if y < ymin, 0 otherwise
```

### Region Codes:

```
1001 = TOP-LEFT        (both outside)
1000 = TOP             (outside top)
1010 = TOP-RIGHT       (both outside)
0001 = LEFT            (outside left)
0000 = INSIDE          (inside window)
0010 = RIGHT           (outside right)
0101 = BOTTOM-LEFT     (both outside)
0100 = BOTTOM          (outside bottom)
0110 = BOTTOM-RIGHT    (both outside)
```

---

## 🔢 Calculate Outcode

### Algorithm:

```
function calculateOutcode(x, y, xmin, xmax, ymin, ymax):
    code = 0
    
    if (x < xmin):
        code |= LEFT    (set bit 1)
    
    if (x > xmax):
        code |= RIGHT   (set bit 0)
    
    if (y < ymin):
        code |= BOTTOM  (set bit 3)
    
    if (y > ymax):
        code |= TOP     (set bit 2)
    
    return code
```

### Example:

```
Window: (10,10) to (100,100)

Point A(5, 50):
  x=5 < 10 (xmin)? YES → LEFT = 0001
  Result: 0001

Point B(50, 120):
  y=120 > 100 (ymax)? YES → TOP = 1000
  Result: 1000

Point C(50, 50):
  Inside window
  Result: 0000
```

---

## ✅ Cohen-Sutherland Steps

### Step 1: Calculate Outcodes

```
For endpoints P1(x1,y1) and P2(x2,y2):
  code1 = calculateOutcode(x1, y1, xmin, xmax, ymin, ymax)
  code2 = calculateOutcode(x2, y2, xmin, xmax, ymin, ymax)
```

### Step 2: Visibility Check

```
if (code1 == 0 AND code2 == 0):
    ACCEPT line (both endpoints inside)
    return

if ((code1 & code2) != 0):
    REJECT line (both endpoints on same outside side)
    return

    (Bitwise AND: if result ≠ 0, both points share
     at least one outside region)
```

### Step 3: Find Intersections

If line is partially visible, find where it crosses window edges:

```
For each window edge (top, bottom, left, right):
  if (endpoint is outside this edge):
    Calculate intersection point of line with edge
    Replace endpoint with intersection point
    Recalculate outcode
    Go to Step 2
```

### Step 4: Draw Clipped Line

```
After clipping, line endpoints are inside window
Draw line segment
```

---

## 🧮 Find Intersection Formula

### When Line Crosses Vertical Edge (LEFT or RIGHT):

```
If crossing LEFT (x = xmin):
  x_intersect = xmin
  y_intersect = y1 + (y2 - y1) × (xmin - x1) / (x2 - x1)

If crossing RIGHT (x = xmax):
  x_intersect = xmax
  y_intersect = y1 + (y2 - y1) × (xmax - x1) / (x2 - x1)
```

### When Line Crosses Horizontal Edge (TOP or BOTTOM):

```
If crossing BOTTOM (y = ymin):
  y_intersect = ymin
  x_intersect = x1 + (x2 - x1) × (ymin - y1) / (y2 - y1)

If crossing TOP (y = ymax):
  y_intersect = ymax
  x_intersect = x1 + (x2 - x1) × (ymax - y1) / (y2 - y1)
```

---

## 📋 Pseudocode

```
function cohenSutherlandClip(x1, y1, x2, y2, xmin, xmax, ymin, ymax):
    
    while (true):
        code1 = calculateOutcode(x1, y1, xmin, xmax, ymin, ymax)
        code2 = calculateOutcode(x2, y2, xmin, xmax, ymin, ymax)
        
        if (code1 == 0 AND code2 == 0):
            ACCEPT (both inside)
            break
        
        if ((code1 & code2) != 0):
            REJECT (both outside same side)
            break
        
        // Line is partially visible
        code = code1
        if (code == 0):
            code = code2  // Use the outside endpoint
            swap(x1,x2)
            swap(y1,y2)
        
        // Find intersection
        if (code & TOP):
            x_intersect = x1 + (x2 - x1) × (ymax - y1) / (y2 - y1)
            y_intersect = ymax
        
        else if (code & BOTTOM):
            x_intersect = x1 + (x2 - x1) × (ymin - y1) / (y2 - y1)
            y_intersect = ymin
        
        else if (code & RIGHT):
            x_intersect = xmax
            y_intersect = y1 + (y2 - y1) × (xmax - x1) / (x2 - x1)
        
        else if (code & LEFT):
            x_intersect = xmin
            y_intersect = y1 + (y2 - y1) × (xmin - x1) / (x2 - x1)
        
        // Replace endpoint with intersection
        x1 = x_intersect
        y1 = y_intersect
```

---

## 🎯 Cohen-Sutherland Example 1: Line Partially Outside

### Setup:
```
Window: (30,30) to (70,70)
Line: P1(10, 50) to P2(80, 50)
```

### Step 1: Calculate Outcodes

```
P1(10, 50):
  x=10 < 30 (xmin)? YES → LEFT = 0001
  code1 = 0001

P2(80, 50):
  x=80 > 70 (xmax)? YES → RIGHT = 0010
  code2 = 0010
```

### Step 2: Visibility Check

```
code1 = 0001, code2 = 0010
code1 == 0? NO
code2 == 0? NO

(code1 & code2) = 0001 & 0010 = 0000
Result ≠ 0? NO → Line is partially visible
Need to clip
```

### Step 3: Find Intersections

```
Point P1(10, 50) is outside LEFT:
  Intersection with x = xmin = 30:
  y = 50 + (50-50) × (30-10)/(80-10)
  y = 50 + 0 = 50
  Intersection: (30, 50)
  Update P1 to (30, 50)

Point P2(80, 50) is outside RIGHT:
  Intersection with x = xmax = 70:
  y = 50 + (50-50) × (70-10)/(80-10)
  y = 50 + 0 = 50
  Intersection: (70, 50)
  Update P2 to (70, 50)
```

### Step 4: Verify Clipped Line

```
New line: (30, 50) to (70, 50)
Both endpoints inside window ✓

Visual:
●────────────┌────●────┐────────●
(10,50)     (30,50) (70,50)   (80,50)
            └─ Window ─┘
            Draw this part
```

### Result:
```
Clipped Line: (30, 50) to (70, 50)
Action: Draw
```

---

## 🎯 Cohen-Sutherland Example 2: Line Entering and Exiting

### Setup:
```
Window: (20,20) to (80,80)
Line: P1(10, 10) to P2(90, 90)
```

### Step 1: Calculate Outcodes

```
P1(10, 10):
  x=10 < 20? YES → LEFT
  y=10 < 20? YES → BOTTOM
  code1 = 0101 (BOTTOM-LEFT)

P2(90, 90):
  x=90 > 80? YES → RIGHT
  y=90 > 80? YES → TOP
  code2 = 1010 (TOP-RIGHT)
```

### Step 2: Visibility Check

```
(code1 & code2) = 0101 & 1010 = 0000
No common bits → Line is partially visible
Need to clip both endpoints
```

### Step 3: Find Intersection for P1

```
P1 code = 0101 (outside both LEFT and BOTTOM)
Priority: Check LEFT first

Intersection with LEFT (x = 20):
  y = 10 + (90-10) × (20-10) / (90-10)
  y = 10 + 80 × 10 / 80
  y = 10 + 10 = 20
  New P1: (20, 20)
```

### Step 4: Verify P1 Inside

```
(20, 20) is on corner of window
New code1 = 0000 → Inside
Continue to P2
```

### Step 5: Find Intersection for P2

```
P2 code = 1010 (outside both RIGHT and TOP)
Priority: Check RIGHT first

Intersection with RIGHT (x = 80):
  y = 10 + (90-10) × (80-10) / (90-10)
  y = 10 + 80 × 70 / 80
  y = 10 + 70 = 80
  New P2: (80, 80)
```

### Step 6: Final Line

```
Original:  (10,10) to (90,90)
Clipped:   (20,20) to (80,80)

Visual:
        (10,10)
        ●
       ╱
      ╱        ┌────────────┐
     ╱  ●──────●─────────● ╱
    ╱  (20,20)      (80,80)
   ╱             │
  ╱              ├─ Clipped line
 ╱               │
●────────────────●
(90,90)    (draw this)

Window: ┌────────────┐
        │            │
        │  (20,20)   │
        │     to     │
        │  (80,80)   │
        │            │
        └────────────┘
```

### Result:
```
Clipped Line: (20, 20) to (80, 80)
Action: Draw
```

---

