# Scaling in Computer Graphics

Scaling is a geometric transformation used to change the size of an object.

## Types of Scaling

- **Uniform scaling**: the same scale factor is applied on all axes.
- **Non-uniform scaling**: different scale factors are applied on different axes.

## Scaling Matrix

For a 2D point `(x, y)`, scaling by factors `Sx` and `Sy` is given by:

```text
[ x' ]   [ Sx  0  ] [ x ]
[ y' ] = [ 0   Sy ] [ y ]
```

For 3D, the matrix is:

```text
[ x' ]   [ Sx  0   0 ] [ x ]
[ y' ] = [ 0   Sy  0 ] [ y ]
[ z' ]   [ 0   0   Sz] [ z ]
```

## Effects

- If scale factor > 1, the object expands.
- If scale factor < 1, the object shrinks.
- If scale factor = 1, the object remains unchanged.
- Negative scale factors can reflect the object about an axis.

## About a Fixed Point

To scale an object about a fixed point `(xf, yf)`, use:

1. Translate the object so the fixed point is at the origin.
2. Apply scaling.
3. Translate back to the original position.

## Applications

- Zooming in graphics
- Resizing images and objects
- Changing model dimensions in CAD systems

![Scaling_Example](image/scaling.png)
Optimizing tool selection...## 2D Scaling Example

**Given:**
- Original point: $A = (x, y) = (2, 3)$
- Scaling factors: $S_x = 2$ (scale x by 2), $S_y = 3$ (scale y by 3)

**Formula for 2D scaling:**
$$x' = x \times S_x, \quad y' = y \times S_y$$

**Solution:**
$$x' = 2 \times 2 = 4$$
$$y' = 3 \times 3 = 9$$

**Result:**
$$A' = (x', y') = (4, 9)$$

**Matrix Form:**
$$A' = \begin{bmatrix} S_x & 0 \\ 0 & S_y \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix} = \begin{bmatrix} 2 & 0 \\ 0 & 3 \end{bmatrix} \begin{bmatrix} 2 \\ 3 \end{bmatrix} = \begin{bmatrix} 4 \\ 9 \end{bmatrix}$$

**Meaning:**
- The point moves from (2, 3) to (4, 9)
- x-coordinate doubled (multiplied by 2)
- y-coordinate tripled (multiplied by 3)
- The object appears **larger** (scaling factors > 1)

**Note:** If scaling factors are < 1, the object shrinks. If scaling factors are = 1, there's no change.