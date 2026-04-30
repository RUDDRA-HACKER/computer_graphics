# 2D Viewing Transformation (Window & Viewport Notes)

---

## 🎯 Definition

Viewing transformation converts **world coordinates → screen coordinates**.

---

## 🧠 Basic Flow

```
World → Window → Viewport → Screen
```

---

# � Complete Viewing Pipeline Steps

## Step 1️⃣: **Object Modeling** (Modeling Coordinates)

### What Happens:
- Objects are created in **object/local coordinates**
- Each object has its own coordinate system
- Example: A circle defined from (-1,-1) to (1,1)

### Output:
```
Object Coordinates → Local representation
```

---

## Step 2️⃣: **Modeling Transformation** (Transform to World)

### What Happens:
- Objects are **scaled, rotated, translated** to world space
- Position and orient objects in the scene
- Multiple objects placed in one world

### Operations:
```
Translation: Move object (x+dx, y+dy)
Rotation: Rotate around a point
Scaling: Enlarge or shrink
```

### Output:
```
World Coordinates → Positioned in scene
```

---

## Step 3️⃣: **Viewing Transformation** (World → Window)

### What Happens:
- Select **WHAT part of world** to display
- Define window in world coordinates
- Only objects inside window are processed

### Window Parameters:
```
xwmin, xwmax = Left and right edges
ywmin, ywmax = Bottom and top edges
```

### Output:
```
Window Coordinates → Normalized space
```

---

## Step 4️⃣: **Clipping** (Remove Outside Objects)

### What Happens:
- Remove all objects **OUTSIDE the window**
- Clip edges of objects that cross window boundary
- Keep only visible parts

### Clipping Algorithms:
```
Cohen-Sutherland  (lines)
Liang-Barsky      (lines)
Sutherland-Hodgman (polygons)
```

### Output:
```
Clipped Objects → Inside window only
```

---

## Step 5️⃣: **Viewport Transformation** (Viewport Mapping)

### What Happens:
- Map window contents to **screen viewport**
- Stretch or shrink to fit viewport size
- Use the transformation formula

### Transformation Formula:
$$x_v = x_{vmin} + \frac{(x_w - x_{wmin})(x_{vmax} - x_{vmin})}{(x_{wmax} - x_{wmin})}$$

### Viewport Parameters:
```
xvmin, xvmax = Left and right boundaries (pixels)
yvmin, yvmax = Bottom and top boundaries (pixels)
```

### Output:
```
Screen Coordinates → Device coordinates (pixels)
```

---

## Step 6️⃣: **Rasterization** (Pixels on Screen)

### What Happens:
- Convert vector graphics → **pixel representation**
- Determine which pixels to fill
- Assign colors to pixels

### Scanline Algorithm:
```
For each horizontal line on screen:
  Determine which objects intersect
  Fill pixels between intersections
  Apply colors/shading
```

### Output:
```
Pixel Data → Display on monitor
```

---

## Step 7️⃣: **Display** (Final Output)

### What Happens:
- Send pixel data to **graphics hardware**
- Render on monitor
- Show final image

### Output:
```
Screen Display → User sees the image 👁️
```

---

# 📈 Complete Pipeline Visual

```
┌─────────────────────────────────────────────────────────┐
│ STEP 1: Object Modeling (Local Coordinates)             │
│ Example: Circle at (-1,-1) to (1,1)                     │
└────────────────────────┬────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│ STEP 2: Modeling Transformation (Translate/Rotate)     │
│ Transform: Move circle to (100,100), scale 2x          │
└────────────────────────┬────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│ STEP 3: Viewing Transformation (Select Window)         │
│ Window: (50,50) to (250,250)                           │
│ What part of world to see                              │
└────────────────────────┬────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│ STEP 4: Clipping (Remove Outside Objects)              │
│ Keep only parts inside window                          │
└────────────────────────┬────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│ STEP 5: Viewport Transformation (Map to Screen)        │
│ Viewport: (0,0) to (500,500) pixels                    │
│ Where to display on screen                             │
└────────────────────────┬────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│ STEP 6: Rasterization (Convert to Pixels)              │
│ Determine which pixels to light up                     │
└────────────────────────┬────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│ STEP 7: Display (Show on Monitor)                       │
│ Final image appears on screen                          │
└─────────────────────────────────────────────────────────┘
```

---

# 🔄 Data Flow Through Pipeline

| Step | Input | Operation | Output |
|------|-------|-----------|--------|
| 1 | Object specs | Define shape | Local coords |
| 2 | Local coords | Transform | World coords |
| 3 | World coords | Select area | Window coords |
| 4 | Window coords | Clip edges | Visible parts |
| 5 | Visible parts | Map to screen | Screen coords |
| 6 | Screen coords | Draw pixels | Pixel data |
| 7 | Pixel data | Display | Screen image |

---

# 💡 Key Relationships

### Window ← → Viewport:
```
Window = WHAT you see (world space)
         ↓ (transformation formula)
Viewport = WHERE you see (screen space)
```

### Transformation Math:
```
Screen = Viewport_min + ((World - Window_min) × 
                        (Viewport_size / Window_size))
```

### Aspect Ratio Important:
```
If Window_AR = Viewport_AR → Objects look normal
If Window_AR ≠ Viewport_AR → Objects appear stretched/compressed
```

---

# 🎯 Example: Complete Pipeline

### Given Setup:
```
Object: Rectangle at (0,0) to (2,2) in local coords

Modeling Transform:
  Scale by 50 → (0,0) to (100,100)
  Translate by (75,75) → (75,75) to (175,175)

World View:
  Window: (50,50) to (200,200)
  Viewport: (0,0) to (800,800)
```

### Through Pipeline:
```
1. Local: Rectangle (0,0)-(2,2)
          ↓
2. World: Rectangle (75,75)-(175,175)
          ↓
3. Window: Rectangle (75,75)-(175,175) ← All visible
          ↓
4. Clipped: Rectangle (75,75)-(175,175) ← All kept
          ↓
5. Screen: Transformation applied
          x_screen = 0 + (x_world - 50) × (800/150)
          ← Rectangle at screen pixels
          ↓
6. Pixels: Draw filled rectangle
          ↓
7. Display: Show on monitor ✓
```

---

# 🧠 Remember:

```
MODELING  → Objects in local coords
   ↓
TRANSFORM → Place in world
   ↓
VIEWING   → Select window region
   ↓
CLIPPING  → Remove outside parts
   ↓
VIEWPORT  → Map to screen area
   ↓
RASTERIZE → Draw pixels
   ↓
DISPLAY   → Show image
```

---
# 🔪 Clipping Window - Complete Guide

## 🎯 Definition

**Clipping** is the process of determining which parts of a graphic primitive (points, lines, polygons) are inside or outside the viewing window, and removing (clipping off) the parts that fall outside.

---

## 📌 Why Clipping is Needed

### Problems Without Clipping:
```
❌ Draw all objects even outside window
❌ Waste processing power
❌ Cause rendering errors
❌ Display artifacts at edges
```

### Benefits of Clipping:
```
✅ Only process visible parts
✅ Faster rendering
✅ Clean display
✅ Improve performance
```

---

## 🔲 Clipping Window Boundaries

### Window Definition:
```
xwmin ─────────────── xwmax
  │                     │
  │   VISIBLE AREA      │
  │   (Inside window)   │
  │                     │
ywmin ─────────────── ymax
```

### Test Condition:
```
Point (x, y) is INSIDE if:
  xwmin ≤ x ≤ xwmax  AND  ywmin ≤ y ≤ ywmax
  
Point is OUTSIDE if:
  x < xwmin  OR  x > xwmax  OR  y < ywmin  OR  y > ywmax
```

---

## 🔢 Types of Clipping

### 1️⃣ **Point Clipping**

**Definition:** Determine if a point is inside or outside the window.

**Algorithm:**
```
if (x >= xwmin AND x <= xwmax AND 
    y >= ywmin AND y <= ywmax)
    POINT IS INSIDE
else
    POINT IS OUTSIDE
```

**Example:**
```
Window: (10,10) to (100,100)

Point A: (50, 50)  → INSIDE ✓
Point B: (5, 50)   → OUTSIDE (x < xwmin)
Point C: (150, 50) → OUTSIDE (x > xwmax)
```

---

### 2️⃣ **Line Clipping**

**Definition:** Clip a line segment so only the part inside the window is visible.

**Cases:**
```
Case 1: Entire line inside  → Keep entire line
        ──────────
        
Case 2: Entire line outside → Discard entire line
        
Case 3: Line crosses window → Keep only inside part
        ─────┃━━━ ━━┃─────
        Keep ┃      ┃
             └──────┘
```

**Algorithms:**

### **Cohen-Sutherland Algorithm**

**Concept:** Divide space into regions

```
        1001  1000  1010
          │     │      │
        ──┼─────┼──────┼──
      0001| 0000│  0010│
        ──┼─────┼──────┼──
          │     │      │
        0101  0100  0110

Regions defined by outcode:
- Top:    1xxx
- Bottom: 0xxx
- Right:  xx1x
- Left:   xx0x

0000 = Inside window
```

**Steps:**
```
Step 1: Assign outcode to each endpoint
Step 2: Check if line is completely inside/outside
Step 3: If partial, find intersection with window edges
Step 4: Recursively clip new line segment
```

**Example:**
```
Line from (5, 50) to (150, 50)
Window: (10,10) to (100,100)

1. Outcode for (5,50):   0001 (Left)
2. Outcode for (150,50): 0010 (Right)
3. Line crosses → Find intersections
4. Keep: (10, 50) to (100, 50)
```

---

### **Liang-Barsky Algorithm**

**Concept:** Parametric line representation

```
Line equation: P(t) = P1 + t(P2 - P1)
where t ∈ [0, 1]

For clipping: Find t_min and t_max
that define the visible segment
```

**Advantages:**
```
✅ More efficient than Cohen-Sutherland
✅ Works for 2D and 3D
✅ Fewer intersection calculations
```

---

### 3️⃣ **Polygon Clipping**

**Definition:** Clip a polygon to the window boundaries.

**Sutherland-Hodgman Algorithm:**

```
Process:
1. Clip against LEFT edge
2. Clip against RIGHT edge
3. Clip against BOTTOM edge
4. Clip against TOP edge

Result: Clipped polygon
```

**Example:**
```
Original Polygon:     After Clipping:
    A                      A'
   ╱ ╲                    ╱ ╲
  ╱   ╲                  ╱   ╲
 B─────C    Window     B'─────C'
  ╲   ╱    ┌─────┐       ╲   ╱
   ╲ ╱     │     │        ╲ ╱
    D      └─────┘         D'
```

---

## 📊 Clipping Comparison Table

| Algorithm | Type | Use Case | Complexity |
|-----------|------|----------|-----------|
| **Point Clipping** | Point | Simple point tests | Simple |
| **Cohen-Sutherland** | Line | 2D line clipping | Medium |
| **Liang-Barsky** | Line | 2D/3D line clipping | Medium-High |
| **Sutherland-Hodgman** | Polygon | Polygon clipping | High |

---

## 🔄 Clipping Workflow

```
┌─────────────────────────────────────┐
│ Object from World Space             │
└──────────────┬──────────────────────┘
               ↓
┌─────────────────────────────────────┐
│ Check Object Type                   │
├─────────┬─────────────┬─────────────┤
│ Point   │ Line        │ Polygon     │
└─────────┼─────────────┼─────────────┘
    ↓           ↓             ↓
┌──────┐  ┌───────────┐  ┌─────────────┐
│Point │  │Cohen-SH or│  │Sutherland- │
│Test  │  │Liang-B    │  │Hodgman    │
└──────┘  └───────────┘  └─────────────┘
    ↓           ↓             ↓
├─────────────────────────────────────┤
│ Keep only INSIDE parts              │
└──────────────┬──────────────────────┘
               ↓
┌─────────────────────────────────────┐
│ Pass to Viewport Transformation     │
└─────────────────────────────────────┘
```

---

## 💡 Visual Example: Line Clipping

```
SCENARIO: Window (30,30) to (70,70)

Original Lines:
1. Line A: (10,50) to (80,50)
   ├─ Starts OUTSIDE (left)
   ├─ Ends OUTSIDE (right)
   └─ Crosses window
   ✂️ CLIPPED to: (30,50) to (70,50)

2. Line B: (50,10) to (50,90)
   ├─ Starts OUTSIDE (below)
   ├─ Ends OUTSIDE (above)
   └─ Crosses window
   ✂️ CLIPPED to: (50,30) to (50,70)

3. Line C: (40,40) to (60,60)
   ├─ Starts INSIDE
   ├─ Ends INSIDE
   └─ Completely in window
   ✅ KEEP ENTIRE LINE

Visual:
      │        B2
      │        │
    ──┼────────┼──
    A1│A──────A2│
      │        │
    ──┼────────┼──
      │        B1
      │
```

---

## ⚙️ Cohen-Sutherland Detailed Steps

### Step 1: Calculate Outcode

```
outcode = 0000

if (y > ymax)   outcode |= 1000 (TOP)
if (y < ymin)   outcode |= 0100 (BOTTOM)
if (x > xmax)   outcode |= 0010 (RIGHT)
if (x < xmin)   outcode |= 0001 (LEFT)
```

### Step 2: Check Endpoints

```
if (outcode1 == 0000 AND outcode2 == 0000)
    → Entire line INSIDE, accept and draw
    
if (outcode1 & outcode2 ≠ 0000)
    → Entire line OUTSIDE, reject
    
else
    → Line partially visible, clip it
```

### Step 3: Find Intersection

```
If endpoint is outside, find where line
crosses window edge and replace endpoint

For crossing TOP edge (y = ymax):
    x = x1 + (x2-x1)×(ymax-y1)/(y2-y1)
    y = ymax
```

---

## 🧠 Key Points About Clipping

1. **Essential Step** - Removes invisible geometry
2. **Improves Performance** - Reduces pixels to process
3. **Prevents Errors** - Avoids rendering artifacts
4. **Algorithm Choice** - Depends on primitive type
5. **Window-Centric** - All clipping relative to window boundaries

---

## 📌 Practical Example

### Setup:
```
Window: (0,0) to (100,100)
Line: (-50,50) to (150,50)
```

### Cohen-Sutherland Solution:

```
1. Outcode for (-50,50):
   x = -50 < 0     → LEFT bit = 1
   Outcode = 0001

2. Outcode for (150,50):
   x = 150 > 100   → RIGHT bit = 1
   Outcode = 0010

3. Not parallel (different codes) → Clip

4. Find intersections:
   Left edge (x=0): Point (-50,50) to (0,50)
   Right edge (x=100): Point (150,50) to (100,50)

5. Clipped line: (0,50) to (100,50) ✓
```

---
# �🟢 Window (Viewing Area)

## 📌 Meaning

```
Window = selected area of the world you want to see
```

---

## ❌ Common Mistake

```
Window ≠ object size
```

✔ Correct:

```
Window = coordinate region (rectangle)
```

---

## 🧠 Example

```
World = 0 to 1000
Window = 200 to 400
```

👉 Only this part is visible.

---

## 🧠 Analogy

```
Window = camera frame 📷
```

---

# 🔵 Viewport (Display Area)

## 📌 Meaning

```
Viewport = area on screen where window is displayed
```

---

## 🧠 Example

```
Viewport = (0,0) to (500,500)
```

👉 Window is mapped into this area.

---

## 🧠 Analogy

```
Viewport = TV screen 📺
```

---

# ⚖️ Window vs Viewport

| Concept  | Meaning           |
| -------- | ----------------- |
| Window   | What you select   |
| Viewport | Where you display |

---

# 🔥 Viewing Transformation Formula

## X-coordinate

$$x_v = x_{vmin} + \frac{(x_w - x_{wmin})(x_{vmax}-x_{vmin})}{(x_{wmax}-x_{wmin})}$$

---

## Y-coordinate

$$y_v = y_{vmin} + \frac{(y_w - y_{wmin})(y_{vmax}-y_{vmin})}{(y_{wmax}-y_{wmin})}$$

---

# 🧠 Formula Breakdown

### Step 1: Shift

```
(xw - xwmin)
```

Moves origin to 0

---

### Step 2: Scale

```
(xvmax - xvmin) / (xwmax - xwmin)
```

Adjusts size

---

### Step 3: Shift to screen

```
+ xvmin
```

Places point on screen

---

## 🧠 Memory Trick

```
Subtract → Multiply → Add
```

---

# 🔢 Example

## Given:

```
Window: (0,0) to (100,100)
Viewport: (0,0) to (500,500)
Point: (50,50)
```

---

## Calculation:

```
Step 1: 50 - 0 = 50
Step 2: Scale = 500/100 = 5 → 50 × 5 = 250
Step 3: +0 → 250
```

---

## ✅ Result

```
(50,50) → (250,250)
```

---

# 🧠 Key Points

* Window selects area from world
* Viewport displays it on screen
* Viewport can scale (zoom effect)
* Shape proportions are maintained

---

# 🎯 Final Summary

```
Window = WHAT you see
Viewport = WHERE you see
Transformation = shift + scale + display
```

---

# 🧠 Super Simple Understanding

```
World = big space 🌍
Window = selected part 📷
Viewport = screen display 📺
```
