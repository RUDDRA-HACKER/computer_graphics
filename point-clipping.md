# Point Clipping Algorithm - Complete Guide

---

## 🎯 Definition

**Point Clipping** is the process of determining whether a given point lies inside or outside the clipping window (viewport). If the point is inside, it's kept (accepted); if outside, it's discarded (rejected).

---

## 🧠 Conceptual Overview

### What is Point Clipping?

```
Given:
  - A point P(x, y) in world coordinates
  - A rectangular clipping window

Determine:
  - Is point P inside the window? → ACCEPT (Draw)
  - Is point P outside the window? → REJECT (Don't draw)
```

### Why Point Clipping?

```
✅ Simple and fastest clipping operation
✅ Foundation for line and polygon clipping
✅ Used in interactive graphics (mouse selection)
✅ Basic operation in rendering pipeline
```

---

## 🔲 Clipping Window Definition

### Window Parameters:

```
xwmin ─────────────────── xwmax
  │                         │
  │    WINDOW              │
  │  (Accept Region)       │
  │                         │
ywmax ─────────────────── ywmin
```

### Four Boundaries:

```
LEFT:   x = xwmin
RIGHT:  x = xwmax
BOTTOM: y = ywmin
TOP:    y = ywmax
```

---

## ✅ Point Clipping Condition

### Mathematical Test:

A point P(x, y) is **INSIDE** the window if:

```
xwmin ≤ x ≤ xwmax  AND  ywmin ≤ y ≤ ywmax
```

### Alternative Representation:

```
if ( (x >= xwmin) AND (x <= xwmax) AND
     (y >= ywmin) AND (y <= ywmax) )
{
    ACCEPT point (point is inside)
}
else
{
    REJECT point (point is outside)
}
```

---

## 🔢 Algorithm Steps

### Step-by-Step Procedure:

```
Input: Point P(x, y), Window boundaries (xwmin, xwmax, ywmin, ywmax)
Output: Accept or Reject the point

ALGORITHM PointClipping:

Step 1: Read point coordinates (x, y)
Step 2: Read window boundaries
        xwmin, xwmax, ywmin, ywmax

Step 3: Check X-coordinate
        if (x < xwmin OR x > xwmax)
            Print "Point REJECTED (outside X)"
            Return

Step 4: Check Y-coordinate
        if (y < ywmin OR y > ywmax)
            Print "Point REJECTED (outside Y)"
            Return

Step 5: Both coordinates inside
        Print "Point ACCEPTED (inside window)"
        Return
```

---

## 💻 Pseudocode

### Method 1: Sequential Checks

```
function pointClipping(x, y, xwmin, xwmax, ywmin, ywmax):
    
    if (x < xwmin):
        return "REJECT"    // Left of window
    
    if (x > xwmax):
        return "REJECT"    // Right of window
    
    if (y < ywmin):
        return "REJECT"    // Below window
    
    if (y > ywmax):
        return "REJECT"    // Above window
    
    return "ACCEPT"        // Inside window
```

### Method 2: Combined Condition

```
function pointClipping(x, y, xwmin, xwmax, ywmin, ywmax):
    
    if ((x >= xwmin) AND (x <= xwmax) AND
        (y >= ywmin) AND (y <= ywmax)):
        return "ACCEPT"
    else:
        return "REJECT"
```

### Method 3: Using Flags

```
function pointClipping(x, y, xwmin, xwmax, ywmin, ywmax):
    
    inside = true
    
    if (x < xwmin OR x > xwmax):
        inside = false
    
    if (y < ywmin OR y > ywmax):
        inside = false
    
    if (inside):
        return "ACCEPT"
    else:
        return "REJECT"
```

---

## 📊 Algorithm Complexity

### Time Complexity:
```
O(1) - Constant time
(Only 4 comparisons needed)
```

### Space Complexity:
```
O(1) - Constant space
(No additional data structures)
```

### Why So Fast?
```
✅ Simple comparison operations
✅ No iterations or loops
✅ Direct mathematical test
```

---

## 🎯 Example 1: Basic Point Clipping

### Setup:
```
Window: xwmin=10, xwmax=100, ywmin=20, ywmax=80

Test 5 Points:
```

### Point A: (50, 50)
```
Check: 10 ≤ 50 ≤ 100? YES ✓
Check: 20 ≤ 50 ≤ 80?  YES ✓
Result: ACCEPT (inside window)

Visual:
       │
    ──┼──────────
    (50,50) •
    ──┼──────────
       │
```

### Point B: (5, 50)
```
Check: 10 ≤ 5 ≤ 100?  NO ✗ (5 < 10)
Result: REJECT (left of window)

Visual:
    •(5,50)
       │
    ──┼──────────
    ──┼──────────
       │
```

### Point C: (150, 50)
```
Check: 10 ≤ 150 ≤ 100? NO ✗ (150 > 100)
Result: REJECT (right of window)

Visual:
       │
    ──┼──────────  •(150,50)
    ──┼──────────
       │
```

### Point D: (50, 90)
```
Check: 10 ≤ 50 ≤ 100?  YES ✓
Check: 20 ≤ 90 ≤ 80?   NO ✗ (90 > 80)
Result: REJECT (above window)

Visual:
              •(50,90)
       │
    ──┼──────────
    ──┼──────────
       │
```

### Point E: (50, 10)
```
Check: 10 ≤ 50 ≤ 100?  YES ✓
Check: 20 ≤ 10 ≤ 80?   NO ✗ (10 < 20)
Result: REJECT (below window)

Visual:
       │
              •(50,10)
    ──┼──────────
    ──┼──────────
       │
```

---

## 🎯 Example 2: Multiple Points Grid

### Setup:
```
Window: (30,30) to (70,70)

Test 9 points in 3×3 grid:
```

### Visual Grid:

```
         20    50    80
         │     │     │
    80───┼─────┼─────┼───
         │     │     │
    60───┼─────●─────┼───   • = Test point
         │     │     │       Inside window
    50───┼─────●─────┼───   ┌──────────┐
         │ ┌───┼──────┼┐    │ Window   │
    40───●─┼───●─────●┤    │ 30 to 70 │
         │ │   │     │ │    └──────────┘
    30───●─┼───●─────●┤
         │ │   │     │ │
    20───┼─┴───┼─────┼─┘
         │     │     │
         20    50    80
```

### Test Results:

| Point | Coordinates | x-Check | y-Check | Result |
|-------|-------------|---------|---------|--------|
| 1 | (20,20) | ✗ | ✗ | REJECT |
| 2 | (50,20) | ✓ | ✗ | REJECT |
| 3 | (80,20) | ✗ | ✗ | REJECT |
| 4 | (20,50) | ✗ | ✓ | REJECT |
| 5 | (50,50) | ✓ | ✓ | **ACCEPT** |
| 6 | (80,50) | ✗ | ✓ | REJECT |
| 7 | (20,80) | ✗ | ✗ | REJECT |
| 8 | (50,80) | ✓ | ✗ | REJECT |
| 9 | (80,80) | ✗ | ✗ | REJECT |

**Result: Only 1 point accepted (center point)**

---

## 🔍 Example 3: Detailed Walkthrough

### Scenario:
```
Window: (40, 50) to (160, 150)

Point to test: P(120, 100)
```

### Step-by-Step Execution:

```
Input: x=120, y=100
       xwmin=40, xwmax=160, ywmin=50, ywmax=150

Step 1: Check if x < xwmin
        120 < 40?
        NO (continue)

Step 2: Check if x > xwmax
        120 > 160?
        NO (continue)

Step 3: Check if y < ywmin
        100 < 50?
        NO (continue)

Step 4: Check if y > ywmax
        100 > 150?
        NO (continue)

Step 5: All checks passed
        Return "ACCEPT"

Output: Point (120, 100) is INSIDE the window ✓
```

### Visual Representation:

```
        40       120      160
         │        │        │
    150──┼────────•────────┼──  Top edge
         │        │        │
    100──┼────────●────────┼──  Point here
         │   Window  │        │
     50──┼────────┼────────┼──  Bottom edge
         │        │        │

Result: Point is within all boundaries → ACCEPT
```

---

## 🎓 Example 4: Real-World Application

### Mouse Click Detection in Graphics Editor:

```
Scenario: User clicks on canvas
Window (Selection Area): (100,100) to (400,400)
Mouse Click Position: (250, 300)

Algorithm Execution:
├─ x = 250
├─ Check: 100 ≤ 250 ≤ 400? YES
├─ Check: 100 ≤ 300 ≤ 400? YES
└─ ACCEPT → Draw object/select region

Next Click Position: (500, 300)
├─ x = 500
├─ Check: 100 ≤ 500 ≤ 400? NO (500 > 400)
└─ REJECT → Click outside, ignore
```

---

## 🔄 Algorithm Variations

### Variation 1: Inclusive vs Exclusive Boundaries

**Inclusive (Standard):**
```
if ((x >= xwmin) AND (x <= xwmax) AND
    (y >= ywmin) AND (y <= ywmax))
    ACCEPT
```

**Exclusive:**
```
if ((x > xwmin) AND (x < xwmax) AND
    (y > ywmin) AND (y < ywmax))
    ACCEPT
```

### Variation 2: Early Exit

```
function pointClipping(x, y, xwmin, xwmax, ywmin, ywmax):
    
    // Exit early if outside
    if (x < xwmin OR x > xwmax):
        return "REJECT"
    
    if (y < ywmin OR y > ywmax):
        return "REJECT"
    
    return "ACCEPT"
```

### Variation 3: Outcode Method

```
function pointClipping(x, y, xwmin, xwmax, ywmin, ywmax):
    
    outcode = 0
    
    if (x < xwmin):
        outcode |= 0001 (LEFT)
    
    if (x > xwmax):
        outcode |= 0010 (RIGHT)
    
    if (y < ywmin):
        outcode |= 0100 (BOTTOM)
    
    if (y > ywmax):
        outcode |= 1000 (TOP)
    
    if (outcode == 0000):
        return "ACCEPT"
    else:
        return "REJECT"
```

---

## 📏 Decision Table

### All Possible Conditions:

```
x < xwmin  |  x > xwmax  |  y < ywmin  |  y > ywmax  | Result
-----------|-------------|-------------|-------------|--------
   NO      |     NO      |     NO      |     NO      | ACCEPT ✓
   YES     |     NO      |     -       |     -       | REJECT
    NO     |     YES     |     -       |     -       | REJECT
    -      |     -       |     YES     |     -       | REJECT
    -      |     -       |     NO      |     YES     | REJECT
```

**Summary:** Accept only when ALL four conditions are false

---

## 🎨 Visual Categories

### 9-Region Classification:

```
   1001  1000  1010      Region Codes:
   │     │     │         1001 = Top-Left (OUT)
   ├─────┼─────┤         1000 = Top (OUT)
1001│ 0000│ 0010│1010     1010 = Top-Right (OUT)
   │     │     │         0001 = Left (OUT)
   ├─────┼─────┤         0000 = Center (IN)
0001│     │     │0010     0010 = Right (OUT)
   │     │     │         0101 = Bottom-Left (OUT)
   ├─────┼─────┤         0100 = Bottom (OUT)
0101│ 0100│ 0110│0110     0110 = Bottom-Right (OUT)
   │     │     │
   
Point in region 0000 → ACCEPT
Point in any other region → REJECT
```

---

## 💡 Key Points to Remember

1. **Simplest Clipping Algorithm** - Only requires comparisons
2. **Always Used First** - Before line and polygon clipping
3. **Four Tests Required** - x-min, x-max, y-min, y-max
4. **Fast Execution** - O(1) time complexity
5. **Binary Decision** - Accept or Reject (no partial results)
6. **Boundary Rules** - Can be inclusive or exclusive
7. **No Coordinates Change** - Points are not modified
8. **No Interpolation** - Direct boolean check

---

## ✏️ Practice Examples

### Exercise 1:
```
Window: (0,0) to (100,100)
Point: (50, 150)
Test: Is it inside?

Answer: 0 ≤ 50 ≤ 100? YES
        0 ≤ 150 ≤ 100? NO (150 > 100)
        REJECT ✗
```

### Exercise 2:
```
Window: (-50,-50) to (50,50)
Point: (-25, 0)
Test: Is it inside?

Answer: -50 ≤ -25 ≤ 50? YES
        -50 ≤ 0 ≤ 50? YES
        ACCEPT ✓
```

### Exercise 3:
```
Window: (10,20) to (90,80)
Points: (10,20), (50,50), (90,80), (100,100)
Test: Which are inside?

(10,20):   10≤10≤90? YES, 20≤20≤80? YES → ACCEPT ✓
(50,50):   10≤50≤90? YES, 20≤50≤80? YES → ACCEPT ✓
(90,80):   10≤90≤90? YES, 20≤80≤80? YES → ACCEPT ✓
(100,100): 10≤100≤90? NO → REJECT ✗
```

---

## 🧠 Why Point Clipping is Important

```
1. Foundation Algorithm
   └─ Basis for line clipping (Liang-Barsky, Cohen-Sutherland)
   └─ Basis for polygon clipping

2. Performance
   └─ Fastest clipping method
   └─ Used in interactive applications
   └─ Minimal computation

3. Applications
   └─ Mouse click detection
   └─ Sprite visibility
   └─ Object selection
   └─ Render optimization

4. Learning Tool
   └─ Introduces clipping concepts
   └─ Easy to understand
   └─ Gateway to advanced algorithms
```

---

## 📚 Summary Table

| Aspect | Details |
|--------|---------|
| **Purpose** | Determine if point is inside/outside window |
| **Condition** | `xwmin ≤ x ≤ xwmax AND ywmin ≤ y ≤ ywmax` |
| **Time** | O(1) |
| **Space** | O(1) |
| **Output** | Accept (inside) or Reject (outside) |
| **Use Cases** | Mouse clicks, sprite visibility, selection |
| **Boundaries** | Usually inclusive (≤, ≥) |
| **Regions** | 9 possible regions (1 inside, 8 outside) |

---

## 🎯 Algorithm Flowchart

```
START
  │
  ├─→ Input: Point (x,y), Window (xwmin,xwmax,ywmin,ywmax)
  │
  ├─→ Check: x < xwmin?
  │     YES → REJECT
  │     NO → Continue
  │
  ├─→ Check: x > xwmax?
  │     YES → REJECT
  │     NO → Continue
  │
  ├─→ Check: y < ywmin?
  │     YES → REJECT
  │     NO → Continue
  │
  ├─→ Check: y > ywmax?
  │     YES → REJECT
  │     NO → Continue
  │
  ├─→ All checks passed
  │
  └─→ Output: ACCEPT
     END
```

---

## 🔗 Related Algorithms

1. **Line Clipping**
   - Cohen-Sutherland (uses point clipping concepts)
   - Liang-Barsky (also checks points)

2. **Polygon Clipping**
   - Sutherland-Hodgman (uses point tests)

3. **Window/Viewport Transformation**
   - Determines which points to transform

4. **Visibility Determination**
   - Checks if objects are visible in viewport
