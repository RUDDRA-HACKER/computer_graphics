**2D Rotation: Definition**

A **2D rotation** is a transformation that turns a point or shape in a plane around a **fixed point** by a given **angle**.

- The fixed point is called the **center of rotation**
- The angle tells how much the figure turns
- **Anticlockwise** rotation is usually taken as **positive**
- **Clockwise** rotation is taken as **negative**

**Rotation About the Origin**

If a point is `(x, y)` and we rotate it by angle `θ` about the origin `(0,0)`, the new point is:
$$
x' = x\cos\theta - y\sin\theta
$$

$$
y' = x\sin\theta + y\cos\theta
$$

This can also be written in matrix form:
$$
\begin{bmatrix}
x' \\
y'
\end{bmatrix}
=
\begin{bmatrix}
\cos\theta & -\sin\theta \\
\sin\theta & \cos\theta
\end{bmatrix}
\begin{bmatrix}
x \\
y
\end{bmatrix}
$$

**Common Rotations**

- `90° anticlockwise`: `(x, y) → (-y, x)`
- `180°`: `(x, y) → (-x, -y)`
- `270° anticlockwise`: `(x, y) → (y, -x)`
- `90° clockwise`: `(x, y) → (y, -x)`

**Example 1: Rotate a Point by 90° Anticlockwise**

Rotate point `(2, 3)` about the origin.

Using rule:
$$
(x, y) \to (-y, x)
$$

So:
$$
(2, 3) \to (-3, 2)
$$

**Answer:** The new point is `(-3, 2)`.

**Example 2: Rotate a Point by 180°**

Rotate `(4, -1)` about the origin.

Using rule:
$$
(x, y) \to (-x, -y)
$$

So:
$$
(4, -1) \to (-4, 1)
$$

**Answer:** The rotated point is `(-4, 1)`.

**Rotation About Any Point**

If rotation is about a point `(a, b)` instead of the origin:

1. Shift the point so `(a, b)` becomes the origin
2. Rotate it
3. Shift back

**Properties of 2D Rotation**

- Shape and size do **not** change
- Distances remain the same
- Angles remain the same
- Only the **position** and **direction** change

If you want, I can also give this in **very short exam notes format** or with a **diagram-style explanation**.

+90° → (-y, x)
-90° → (y, -x)
180° → (-x, -y)

| Rotation | Formula  |
| -------- | -------- |
| 90° ACW  | (-y, x)  |
| 90° CW   | (y, -x)  |
| 180°     | (-x, -y) |
| 270° ACW | (y, -x)  |
| 270° CW  | (-y, x)  |
