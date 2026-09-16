# Station Twist as an X-Dependent Matrix Transformation

**Mudfish geometry experiment — spreadsheet-native formulation**  
**Date:** September 16, 2026  
**Source:** ChatGPT (Vale), reviewed and extended by Grok and Claude (Airy)

---

## 1. Core Idea

A gradual twist applied as a function of longitudinal position $x$ is a natural use of a transformation matrix, implementable directly in LibreOffice Calc or Excel using native `SIN()`, `COS()`, and `MMULT()` functions — no VBA or Python required.

Let a point on a hull station at longitudinal position $x$ have coordinates:

$$\mathbf{p} = \begin{bmatrix} x \\ y \\ z \end{bmatrix}$$

Define a twist law along the hull: $\theta = \theta(x)$

---

## 2. Rotation Matrix About the Longitudinal Axis

$$R_x(\theta) = \begin{bmatrix} 1 & 0 & 0 \\ 0 & \cos\theta & -\sin\theta \\ 0 & \sin\theta & \cos\theta \end{bmatrix}$$

The transformed point is:

$$\mathbf{p}' = R_x(\theta(x))\,\mathbf{p}$$

Every station receives its own rotation matrix, but the coefficients are calculated automatically from that station's $x$ coordinate.

---

## 3. Spreadsheet Implementation

```
theta = k*x  →  build R_x(theta)  →  MMULT(R_x, point)
```

The matrix is a parameterized template: change $x$ and the matrix updates automatically. This is a pure **Operator Layer** operation — it does not touch the Formula Layer that generates the base station points.

---

## 4. Rotating About the Station Centre ⚠️ Important

Rotating about the global $x$-axis sends hull geometry orbiting around the origin — **not** the intended behaviour. Instead, define the station centre:

$$C(x) = \begin{bmatrix} x \\ y_c(x) \\ z_c(x) \end{bmatrix}$$

Translate to local coordinates, rotate, translate back:

$$\mathbf{p}' = C(x) + R_x(\theta(x))\,[\mathbf{p} - C(x)]$$

This rotates the geometry about the chosen local centre rather than the global origin.

---

## 5. Homogeneous 4×4 Formulation

The translate → rotate → translate-back operations combine into a single homogeneous $4 \times 4$ matrix $T(x)$, consistent with the Mudfish kernel contract ($P' = P \times M$, row-vector, translation in last row):

$$\mathbf{p}'_h = T(x)\,\mathbf{p}_h$$

The geometry pipeline becomes:

$$\text{Original Station} \;\rightarrow\; T(x) \;\rightarrow\; \text{Twisted Station}$$

This plugs directly into the existing card stack as a dedicated **Twist Card** — a new CHAIN-style card that adds to the stack without touching the formula layer.

---

## 6. Smooth Twist Laws

**Linear (simplest):**
$$\theta(x) = \theta_0 + kx$$

**Cubic smoothstep (preferred for fairness):**

Define a normalized longitudinal parameter:
$$t = \frac{x - x_0}{x_1 - x_0}$$

Then:
$$\theta(x) = \theta_{\max}(3t^2 - 2t^3)$$

This gives zero first derivative at both ends — the twist eases in and out, avoiding kinks. Consistent with the $C^1$ continuity philosophy in `HullContinuityFixture`.

---

## 7. Relationship to Developability

The matrix is the **mechanism**. The engineering question is how $\theta(x)$ should be chosen.

A small, smooth station rotation can be used experimentally to alter the longitudinal streaks while monitoring whether the resulting surface becomes closer to developable.

**Concrete check:** use the triple-product determinant already proven in the Mudfish kernel:

$$(\gamma'(x),\, \mathbf{d}(x),\, \mathbf{d}'(x)) = 0$$

This is the checkable number — not a vague "watch the surface." If the determinant approaches zero, the surface is approaching developability. A later optimization layer (Python only if needed) could search for the smallest $\theta_{\max}$ that minimizes this metric.

---

## 8. Practical Notes for Mudfish

**Coordinate convention:** The locked Mudfish right-hand system (+X forward, +Y port, +Z up) is consistent with this formulation. A positive $\theta$ rotates in the expected direction. Maintain the row-vector convention ($P' = P \times M$) throughout.

**Magnitude warning:** For a 6.5m aluminum hull with pre-punched CNC rivet holes, even a few degrees of total twist can significantly affect plate fairness and rivet-line geometry. Start with very small $\theta_{\max}$ values.

**Implementation order:**
1. Keep current untwisted stations as the reference
2. Add a parallel "Twisted" column or card applying $T(x)$
3. Compare surfaces side-by-side in FreeCAD (or by exporting both point sets)
4. Monitor the triple-product developability metric at each step

---

## 9. Spreadsheet-Native Architecture

**Design principle:** Keep geometry visible and auditable in the spreadsheet. Use macros or Python only where native spreadsheet operations cease to be the clearest tool.

The Twist Card follows the same two-layer separation as all other cards:

| Layer | Content |
|-------|---------|
| Formula Layer (left) | $\theta(x)$ law — linear or smoothstep, driven by named cells |
| Operator Layer (right) | $4 \times 4$ homogeneous $T(x)$ matrix, evaluated via `MMULT` |

---

## Review Summary

| Reviewer | Key contribution |
|----------|-----------------|
| ChatGPT (Vale) | Core mathematics, local-centre formulation, smoothstep law |
| Grok | Architectural integration as Twist Card, coordinate convention check, magnitude warning |
| Claude (Airy) | Triple-product as concrete developability metric, C¹ continuity connection, card-layer mapping |

---

*Part of the Mudfish Matrix Lofting Engine · Sparked Matter LLC · LGPL-3.0 · September 2026*
