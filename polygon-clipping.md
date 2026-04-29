# Polygon Clipping - Complete Guide

---

## 🎯 Definition

**Polygon Clipping** is the process of determining which portions of a polygon lie inside or outside a rectangular clipping window and removing (clipping) the parts that fall outside while keeping the visible parts.

---

## 🧠 Why Polygon Clipping?

### Problems Without Clipping:
```
❌ Draw entire polygons outside viewport
❌ Display parts that shouldn't be visible
❌ Rendering errors at screen edges
❌ Wasted processing on invisible geometry
```

### Benefits:
```
✅ Only draw visible polygon parts
✅ Clean viewport rendering
✅ Improved performance
✅ Correct visual output
```

---

## 📊 Polygon Clipping Cases

### Case 1: Entire Polygon Inside
```
        ┌────────────┐
        │  /\        │
        │ /  \       │  Window
        │/    \      │
        └────────────┘
        
Action: ACCEPT entire polygon
Result: Draw completely
```

### Case 2: Entire Polygon Outside
```
        /\
       /  \
      /    \
     /      \
    
    ┌────────────┐
    │            │  Window
    │            │
    └────────────┘
        
Action: REJECT entire polygon
Result: Don't draw
```

### Case 3: Polygon Partially Visible
```
        /\
       /  \
      ╱────\──────┐
     ╱  ┌───┼──────┤
    ╱   │   │      │
   ╱    │   │      │
        └───┴──────┘
        
Action: CLIP polygon
Result: Keep inside part, remove outside
Output: New polygon(s)
```

---

# 🔪 Sutherland-Hodgman Algorithm

## 🎯 Overview

Sutherland-Hodgman is the most common polygon clipping algorithm. It clips a polygon against each window edge sequentially, producing a new polygon after each edge clipping.

---

## 📐 Basic Concept

### Main Idea:

```
Process the polygon edge by edge against each window edge:

Input Polygon → Clip against LEFT → Intermediate polygon
                        ↓
            Intermediate → Clip against RIGHT → Intermediate
                        ↓
            Intermediate → Clip against BOTTOM → Intermediate
                        ↓
            Intermediate → Clip against TOP → Output polygon
```

---

## 🔄 Algorithm Overview

### Four Edges of Window:

```
1. LEFT edge (x = xmin)
2. RIGHT edge (x = xmax)
3. BOTTOM edge (y = ymin)
4. TOP edge (y = ymax)

For each edge, process all polygon edges
```

### For Each Edge:

```
Input: Polygon (list of vertices)
Process each edge of polygon against window edge

For each edge from V_i to V_{i+1}:
  Check if vertices are inside or outside
  
  Case 1: Both inside → Add V_{i+1}
  Case 2: Inside to Outside → Add intersection point
  Case 3: Outside to Inside → Add intersection, then V_{i+1}
  Case 4: Both outside → Add nothing

Output: New polygon (may have different number of vertices)
```

---

## 📋 Point Inside/Outside Test

For each window edge, determine if point is inside or outside:

```
LEFT edge (x = xmin):
  Inside: x >= xmin
  Outside: x < xmin

RIGHT edge (x = xmax):
  Inside: x <= xmax
  Outside: x > xmax

BOTTOM edge (y = ymin):
  Inside: y >= ymin
  Outside: y < ymin

TOP edge (y = ymax):
  Inside: y <= ymax
  Outside: y > ymax
```

---

## 🧮 Find Intersection Point

When edge crosses window boundary:

```
Edge from P1(x1,y1) to P2(x2,y2)
Window edge: varies (LEFT, RIGHT, BOTTOM, TOP)

Parametric form: P(t) = P1 + t(P2-P1)

For vertical edge (LEFT or RIGHT at x = edge_x):
  t = (edge_x - x1) / (x2 - x1)
  intersection_x = edge_x
  intersection_y = y1 + t × (y2 - y1)

For horizontal edge (BOTTOM or TOP at y = edge_y):
  t = (edge_y - y1) / (y2 - y1)
  intersection_x = x1 + t × (x2 - x1)
  intersection_y = edge_y
```

---

## 📝 Pseudocode

```
function sutherlandHodgmanClip(polygon, xmin, xmax, ymin, ymax):
    
    outputList = polygon  // Start with input polygon
    
    // Clip against each edge
    for each edge (LEFT, RIGHT, BOTTOM, TOP):
        
        if (outputList is empty):
            return empty
        
        inputList = outputList
        outputList = empty
        
        if (inputList has vertices):
            S = last vertex of inputList
            
            for each E in inputList:  // For each vertex
                
                if (E is INSIDE current edge):
                    
                    if (S is OUTSIDE):
                        // Entering: add intersection point
                        intersection = calculateIntersection(S, E, edge)
                        add intersection to outputList
                    
                    // Add current vertex
                    add E to outputList
                
                else:  // E is OUTSIDE
                    
                    if (S is INSIDE):
                        // Exiting: add intersection point
                        intersection = calculateIntersection(S, E, edge)
                        add intersection to outputList
                
                S = E  // Move to next edge
    
    return outputList
```

---

## 🎯 Sutherland-Hodgman Example 1: Simple Square

### Setup:
```
Window: (30,30) to (70,70)
Polygon: Square with vertices
  A(20,20)
  B(80,20)
  C(80,80)
  D(20,80)
```

### Step 1: Clip Against LEFT Edge (x = 30)

```
Vertices: A(20,20) → B(80,20) → C(80,80) → D(20,80) → A

Edge A→B:
  A(20,20): OUTSIDE (x=20 < 30)
  B(80,20): INSIDE (x=80 >= 30)
  Action: Add intersection, then B
  Intersection: x=30, y = 20 + (20-20)/(80-20) × (30-20) = 20
  Add: (30,20), B(80,20)

Edge B→C:
  B(80,20): INSIDE
  C(80,80): INSIDE
  Action: Add C
  Add: C(80,80)

Edge C→D:
  C(80,80): INSIDE
  D(20,80): OUTSIDE
  Action: Add intersection
  Intersection: x=30, y = 80
  Add: (30,80)

Edge D→A:
  D(20,80): OUTSIDE
  A(20,20): OUTSIDE
  Action: Add nothing

Output after LEFT: (30,20), (80,20), (80,80), (30,80)
```

### Step 2: Clip Against RIGHT Edge (x = 70)

```
Input: (30,20), (80,20), (80,80), (30,80)

Edge (30,20)→(80,20):
  (30,20): INSIDE
  (80,20): OUTSIDE
  Action: Add intersection
  Intersection: x=70, y=20
  Add: (70,20)

Edge (80,20)→(80,80):
  (80,20): OUTSIDE
  (80,80): OUTSIDE
  Action: Add nothing

Edge (80,80)→(30,80):
  (80,80): OUTSIDE
  (30,80): INSIDE
  Action: Add intersection, then (30,80)
  Intersection: x=70, y=80
  Add: (70,80), (30,80)

Edge (30,80)→(30,20):
  (30,80): INSIDE
  (30,20): INSIDE
  Action: Add (30,20)
  Add: (30,20)

Output after RIGHT: (70,20), (70,80), (30,80), (30,20)
```

### Step 3: Clip Against BOTTOM Edge (y = 30)

```
Input: (70,20), (70,80), (30,80), (30,20)

Edge (70,20)→(70,80):
  (70,20): OUTSIDE (y=20 < 30)
  (70,80): INSIDE
  Action: Add intersection, then (70,80)
  Intersection: y=30, x = 70
  Add: (70,30), (70,80)

Edge (70,80)→(30,80):
  (70,80): INSIDE
  (30,80): INSIDE
  Action: Add (30,80)
  Add: (30,80)

Edge (30,80)→(30,20):
  (30,80): INSIDE
  (30,20): OUTSIDE
  Action: Add intersection
  Intersection: y=30, x=30
  Add: (30,30)

Edge (30,20)→(70,20):
  (30,20): OUTSIDE
  (70,20): OUTSIDE
  Action: Add nothing

Output after BOTTOM: (70,30), (70,80), (30,80), (30,30)
```

### Step 4: Clip Against TOP Edge (y = 70)

```
Input: (70,30), (70,80), (30,80), (30,30)

Edge (70,30)→(70,80):
  (70,30): INSIDE
  (70,80): OUTSIDE (y=80 > 70)
  Action: Add intersection
  Intersection: y=70, x=70
  Add: (70,70)

Edge (70,80)→(30,80):
  (70,80): OUTSIDE
  (30,80): OUTSIDE
  Action: Add nothing

Edge (30,80)→(30,30):
  (30,80): OUTSIDE
  (30,30): INSIDE
  Action: Add intersection, then (30,30)
  Intersection: y=70, x=30
  Add: (30,70), (30,30)

Edge (30,30)→(70,30):
  (30,30): INSIDE
  (70,30): INSIDE
  Action: Add (70,30)
  Add: (70,30)

Output after TOP: (70,70), (30,70), (30,30), (70,30)
```

### Final Result:

```
Original Polygon: Square from (20,20) to (80,80)
Clipped Polygon: (30,30), (70,30), (70,70), (30,70)
                 = Square from (30,30) to (70,70)

Visual:
Before:               After:
A(20,20)──B(80,20)    ┌──(70,30)──┐
│          │          │            │
│          │    →     │            │
│          │          │            │
D(20,80)──C(80,80)    └──(30,70)──┘

Window: (30,30) to (70,70)
```

---

## 🎯 Sutherland-Hodgman Example 2: Triangle Clipping

### Setup:
```
Window: (40,40) to (80,80)
Triangle:
  A(30, 50)
  B(90, 50)
  C(60, 20)
```

### Clip Against LEFT (x = 40):

```
A(30,50): OUTSIDE
B(90,50): INSIDE
C(60,20): INSIDE

Edge A→B:
  A OUTSIDE, B INSIDE
  Add intersection at x=40: y = 50 + (50-50)/(90-30) × (40-30) = 50
  Add: (40,50), B(90,50)

Edge B→C:
  B INSIDE, C INSIDE
  Add: C(60,20)

Edge C→A:
  C INSIDE, A OUTSIDE
  Add intersection at x=40: y = 20 + (50-20)/(30-60) × (40-60) = 20 - 20 = 0
  Add: (40,0)

Output: (40,50), (90,50), (60,20), (40,0)
```

### Clip Against RIGHT (x = 80):

```
Input: (40,50), (90,50), (60,20), (40,0)

Edge (40,50)→(90,50):
  (40,50): INSIDE
  (90,50): OUTSIDE
  Add intersection at x=80: y=50
  Add: (80,50)

Edge (90,50)→(60,20):
  (90,50): OUTSIDE
  (60,20): INSIDE
  Add intersection at x=80: y = 50 + (20-50)/(60-90) × (80-90) = 50 + 10 = 60
  Add: (80,60), (60,20)

Edge (60,20)→(40,0):
  (60,20): INSIDE
  (40,0): INSIDE
  Add: (40,0)

Edge (40,0)→(40,50):
  (40,0): INSIDE
  (40,50): INSIDE
  Add: (40,50)

Output: (80,50), (80,60), (60,20), (40,0), (40,50)
```

### Continue Clipping BOTTOM and TOP...

```
(After BOTTOM clip at y=40)
Output: vertices adjusted

(After TOP clip at y=80)
Final clipped polygon with all parts inside window
```

---

# 🔗 Other Polygon Clipping Algorithms

## 1. Weiler-Atherton Algorithm

**Characteristics:**
```
✓ More complex than Sutherland-Hodgman
✓ Can produce multiple output polygons
✓ Better for concave polygons
✓ Uses vertex list and edge links
```

**Process:**
```
1. Find all intersection points
2. Create linked lists of polygon and window edges
3. Traverse lists following specific rules
4. Generate output polygons
```

---

## 2. Greiner-Hormann Algorithm

**Characteristics:**
```
✓ Efficient for complex polygons
✓ Works with convex or concave polygons
✓ Fewer iterations than others
✓ Good for multiple polygons
```

---

## 3. Martinez-Rueda Algorithm

**Characteristics:**
```
✓ Handles self-intersecting polygons
✓ Can perform union, intersection, difference
✓ Most general approach
✓ More computationally expensive
```

---

# 📊 Algorithm Comparison

| Algorithm | Complexity | Convex/Concave | Output Polygons | Best For |
|-----------|-----------|----------------|-----------------|----------|
| **Sutherland-Hodgman** | O(n×m) | Both | Single | Simple, fast |
| **Weiler-Atherton** | O(n×m) | Both | Multiple | Complex shapes |
| **Greiner-Hormann** | O(n+m) | Both | Multiple | Efficient |
| **Martinez-Rueda** | O(n×m) | Both | Multiple | General purpose |

---

# 📋 Sutherland-Hodgman Algorithm Properties

### Advantages:
```
✅ Simple to understand and implement
✅ Works for both convex and concave polygons
✅ Works for convex clipping window
✅ Fast and efficient
✅ Handles any number of edges
```

### Disadvantages:
```
❌ Assumes convex clipping window (rectangular is convex)
❌ May produce degenerate polygons
❌ Only produces single output polygon
❌ Inefficient for many small polygons
```

---

# 🧠 Special Cases

## Case 1: Polygon Becomes Degenerate

```
Input: Triangle nearly at window edge
After clipping: Line or point
Handle: Check if output has >= 3 vertices
```

## Case 2: Clipping to Empty Set

```
Input: Polygon completely outside
Output: Empty set (0 vertices)
```

## Case 3: Window Edge Coincides with Polygon Edge

```
Input: Polygon edge exactly on window boundary
Output: Include edge normally
```

---

# 💡 Implementation Details

## Vertex Order Preservation:

```
Important: Maintain counter-clockwise order
Input:  A → B → C → A (CCW)
Output: Keep CCW order during clipping
```

## Edge Direction Matters:

```
When finding intersections, direction of traversal affects
which intersection point is chosen

Always use consistent direction (CCW recommended)
```

## Floating-Point Precision:

```
Risk: Numerical errors in intersection calculation
Solution: 
  - Use appropriate epsilon for comparisons
  - Avoid deep nested operations
  - Check for degenerate results
```

---

# 🎨 Visual Examples

## Example 3: Concave Polygon

```
Input Polygon:        Clipped Result:
    A                      A'
   /│\                    / \
  / │ \                  /   \
 /  │  \        →       B'───E'
B───┼───C              /         \
    │ /              D'─────C'
    │/
    D

Window: ┌──────────┐
        │          │
        └──────────┘
```

---

# 📊 Algorithm Flowchart

```
START
  │
  ├─→ outputList = input polygon
  │
  ├─→ For each window edge (LEFT, RIGHT, BOTTOM, TOP):
  │    
  │    ├─→ inputList = outputList
  │    ├─→ outputList = empty
  │    │
  │    ├─→ For each edge S→E in inputList:
  │    │
  │    │    ├─→ E inside edge?
  │    │    │    YES:
  │    │    │    ├─→ S inside? NO → add intersection
  │    │    │    └─→ add E
  │    │    │
  │    │    │    NO:
  │    │    │    ├─→ S inside? YES → add intersection
  │    │    │
  │    │    └─→ S = E
  │
  ├─→ outputList has vertices?
  │    YES → ACCEPT
  │    NO → REJECT
  │
  └─→ END
```

---

# 📝 Practice Problems

### Problem 1:
```
Window: (20,20) to (80,80)
Rectangle: (10,10), (90,10), (90,90), (10,90)

Find clipped polygon using Sutherland-Hodgman
```

### Problem 2:
```
Window: (0,0) to (100,100)
Triangle: (50,50), (150,50), (50,150)

Clip against all edges and show final vertices
```

### Problem 3:
```
Window: (30,30) to (70,70)
Pentagon: (20,20), (80,20), (90,50), (80,80), (20,80)

Step through clipping against LEFT edge only
```

---

# 🔄 Complete Algorithm Summary

```
SUTHERLAND-HODGMAN ALGORITHM:

Input: Polygon, Clipping window
Output: Clipped polygon

1. For each window edge (4 edges):
   
   2. For each polygon edge:
      
      3. Check both vertices against edge:
         
         Both Inside:    Add second vertex
         Inside→Outside: Add intersection only
         Outside→Inside: Add intersection + vertex
         Both Outside:   Add nothing
   
   4. Output becomes input for next edge

5. Final output = clipped polygon
```

---

# 📚 Key Points to Remember

1. **Iterative Process** - Clips against one edge at a time
2. **Vertex Classification** - Each vertex is inside or outside
3. **Intersection Finding** - Parametric line equation
4. **Order Preservation** - Maintain vertex order
5. **Convex Window** - Algorithm assumes rectangular window
6. **Single Output** - Produces one polygon (may be degenerate)
7. **General** - Works for convex or concave input polygons

---

# 🎯 Real-World Applications

```
1. Computer Graphics
   - Viewport rendering
   - Window clipping
   - Hidden surface removal

2. CAD Systems
   - Geometric clipping
   - Design window operations
   - View filtering

3. Game Development
   - Screen space optimization
   - Collision detection
   - Visibility culling

4. Image Processing
   - Region of interest extraction
   - Image window operations
```
