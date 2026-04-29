# Scaling in 3D (Computer Graphics)

## Definition
3D Scaling is a transformation that changes the size of an object by multiplying the coordinates by scale factors along each axis.

In 3D scaling, the object's size changes along the X, Y, and Z axes independently.

If a point is:
- `P(x, y, z)`

and scaling factors are:
- `Sx` along X-axis
- `Sy` along Y-axis
- `Sz` along Z-axis

then the new point is:
- `P'(x', y', z') = (x × Sx, y × Sy, z × Sz)`

---
![Scaling Example](image/3d%20scaling%20-1.png)
## 3D Scaling Equations

$$
x' = x \times S_x
$$

$$
y' = y \times S_y
$$

$$
z' = z \times S_z
$$

For every vertex of a 3D object, apply the same equations.

---

## Matrix Form (Homogeneous Coordinates)

A 3D point in homogeneous form:

$$
\begin{bmatrix}
x \\
y \\
z \\
1
\end{bmatrix}
$$

Scaling matrix for 3D:

$$
S =
\begin{bmatrix}
S_x & 0 & 0 & 0 \\
0 & S_y & 0 & 0 \\
0 & 0 & S_z & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

After scaling:

$$
\begin{bmatrix}
x' \\
y' \\
z' \\
1
\end{bmatrix}
=
\begin{bmatrix}
S_x & 0 & 0 & 0 \\
0 & S_y & 0 & 0 \\
0 & 0 & S_z & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
z \\
1
\end{bmatrix}
$$

---

## Steps to Solve a 3D Scaling Problem

1. Identify the coordinates of each point of the object in 3D space.
2. Note the scaling factors: `Sx`, `Sy`, and `Sz`.
3. Apply the scaling equations to each point.
4. Find the new coordinates `(x', y', z')` for all points.
5. If the scaling factors are:
   - Greater than 1: Object becomes larger
   - Less than 1: Object becomes smaller
   - Equal to 1: No change in that dimension

---
![Scaling Example](image/3d%20scaling%20-2.png)

## Example According to the Image

Given three points in 3D space:

$$
A(3, 1, 2), \quad B(1, 2, 1), \quad C(0, 1, 1)
$$

Scaling factors:

$$
S_x = 2 \text{ units}, \quad S_y = 1 \text{ unit}, \quad S_z = 3 \text{ units}
$$

Using the scaling equations:

$$
x' = x \times S_x, \quad y' = y \times S_y, \quad z' = z \times S_z
$$

### Find the scaled coordinates

For point `A(3, 1, 2)`:

$$
x' = 3 \times 2 = 6
$$

$$
y' = 1 \times 1 = 1
$$

$$
z' = 2 \times 3 = 6
$$

So,

$$
A'(6, 1, 6)
$$

For point `B(1, 2, 1)`:

$$
x' = 1 \times 2 = 2
$$

$$
y' = 2 \times 1 = 2
$$

$$
z' = 1 \times 3 = 3
$$

So,

$$
B'(2, 2, 3)
$$

For point `C(0, 1, 1)`:

$$
x' = 0 \times 2 = 0
$$

$$
y' = 1 \times 1 = 1
$$

$$
z' = 1 \times 3 = 3
$$

So,

$$
C'(0, 1, 3)
$$

### Final scaled points

The scaled 3D points are:

$$
A'(6, 1, 6), \quad B'(2, 2, 3), \quad C'(0, 1, 3)
$$

---

## Key Points

- 3D scaling changes the size of the object.
- Scaling factors can be different for each axis.
- If `Sx = Sy = Sz`, it is **uniform scaling** (object maintains proportions).
- If `Sx ≠ Sy ≠ Sz`, it is **non-uniform scaling** (object may appear distorted).
- Scaling factor > 1: enlarges the object in that dimension.
- Scaling factor < 1: reduces the object in that dimension.
- Scaling factor = 1: no change in that dimension.

---

## Pseudocode

```text
for each vertex (x, y, z) in object:
	x' = x × Sx
	y' = y × Sy
	z' = z × Sz
```

---

## Difference: 2D vs 3D Scaling

| Property | 2D Scaling | 3D Scaling |
|----------|---|---|
| Coordinates | (x, y) | (x, y, z) |
| Scale factors | Sx, Sy | Sx, Sy, Sz |
| Equations | x' = x × Sx, y' = y × Sy | x' = x × Sx, y' = y × Sy, z' = z × Sz |
| Matrix size | 3×3 | 4×4 |
| Uniform scaling | If Sx = Sy | If Sx = Sy = Sz |
