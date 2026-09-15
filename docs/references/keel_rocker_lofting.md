# Boat Hull Keel Rocker Lofting — Parabolic Curve Formula

Using a parabolic curve formula for boat hull keel rocker lofting allows designers to
mathematically generate a "fair" (perfectly smooth, drag-reducing) centerline profile.
Because parabolas naturally vary their acceleration of curvature, they are often preferred
over simple circular arcs to create highly efficient water entry and exit zones.

This specific formula layout defines a quadratic curve centered at $x = 0$ (where $y = Y_0$)
and is highly versatile across engineering disciplines.

---

## 1. Applications Across Engineering Fields

- **General Mathematics:** Geometrically, this equation defines a *Parabolic Profile* with its
  vertex centered at $x = 0$ (where $y = Y_0$). In numerical analysis, it serves as a
  *Quadratic Interpolation / Shape Function* to smoothly bridge values over a boundary span $L$,
  imposing a flat starting slope.

- **Fluid Mechanics:** Describes a *Parabolic Velocity Profile* found in laminar flow conditions
  (such as Hagen-Poiseuille flow inside a pipe). If $Y_1 = 0$ at the boundary wall due to the
  no-slip condition, $Y_0$ represents the maximum centerline velocity.

- **Thermodynamics & Heat Transfer:** Details a *Parabolic Temperature Distribution* for
  steady-state profiles within bodies experiencing uniform internal heat generation.

- **Structural Engineering:** Explains the *Parabolic Mode Shape* used as an assumed trial
  function or deflection curve in the Rayleigh-Ritz method.

---

## 2. The Core Parabolic Formula

When designing a rocker curve, the bottommost point of the keel (maximum draft) is treated as
the origin $(0,\,0)$, or the vertex of the parabola.

The fundamental formula to find the height ($y$) of the curve at any distance ($x$) from that
lowest point is:

$$\boxed{y = Y_0 - (Y_0 - Y_1)\cdot\left(\frac{x}{L}\right)^2}$$

**Where:**

| Symbol | Meaning |
|--------|---------|
| $y$ | Vertical rise or lift of the rocker at a specific station (measured up from the baseline/lowest point) |
| $x$ | Horizontal distance from the rocker's lowest point to that station |
| $Y_0$ | Baseline starting offset or maximum profile depth at the center vertex |
| $Y_1$ | Final targeted boundary height at the terminal station length |
| $L$ | Total horizontal boundary span length from the center vertex to the end of the section |

---

## 3. Step-by-Step Lofting Calculation

To use this on a real hull design, first establish your constraints:

1. $L_{fore}$ — Horizontal distance from the lowest point to the stem (bow)
2. $H_{fore}$ — Desired total vertical rise (rocker height) at the stem
3. $L_{aft}$ — Horizontal distance from the lowest point to the transom (stern)
4. $H_{aft}$ — Desired total vertical rise at the transom

### Step A: Find the Curvature Constant $k$ for Each Section

Because the formula simplifies to $y = k \cdot x^2$ (when $Y_0 = 0$), solve for $k$ using
the known endpoints:

$$k_{fore} = \frac{H_{fore}}{(L_{fore})^2}$$

$$k_{aft} = \frac{H_{aft}}{(L_{aft})^2}$$

### Step B: Calculate the Offset at Any Station

Once you have $k$, plug in the horizontal distance $x$ of any station:

$$\textbf{Forward stations:} \quad y = k_{fore} \cdot x^2$$

$$\textbf{Aft stations:} \quad y = k_{aft} \cdot x^2$$

---

## 4. Practical Example

**Given:**
- Lowest point of the keel is exactly at amidships
- $L_{fore} = 10\text{ ft}$ (amidships to bow)
- $H_{fore} = 12\text{ in}$ (desired rocker rise at the bow)

**Step 1 — Calculate $k$:**

$$k = \frac{12\text{ in}}{(10\text{ ft})^2} = \frac{12}{100} = 0.12 \;\frac{\text{in}}{\text{ft}^2}$$

**Step 2 — Calculate offsets at each station:**

| Station $x$ (ft) | $y = 0.12 \cdot x^2$ (in) |
|:-----------------:|:-------------------------:|
| 2 | $0.12 \times 4 = 0.48$ |
| 4 | $0.12 \times 16 = 1.92$ |
| 6 | $0.12 \times 36 = 4.32$ |
| 8 | $0.12 \times 64 = 7.68$ |
| 10 (Bow) | $0.12 \times 100 = 12.0$ |

---

## 5. Lofting Tips for Parabolas

### Smooth Transitions

Because both the forward and aft parabolas share a vertex at $(0,\,0)$, their slopes are
exactly zero at the origin:

$$\left.\frac{dy}{dx}\right|_{x=0} = 2kx\big|_{x=0} = 0$$

This mathematically guarantees a perfectly flat, fair, and smooth transition where the two
curves meet at the bottom of the boat — $C^1$ continuity at the keel minimum, no kink.

### Adjusting for Absolute Grid Coordinates

If you prefer your origin $(0,\,0)$ to be at the bow tip rather than the lowest keel point,
the equation shifts into the standard quadratic form:

$$y = ax^2 + bx + c$$

which can be modelled directly using a polynomial regression trendline in any spreadsheet.

---

## Connection to the Mudfish Matrix Lofting Engine

This formula is the **same quadratic interpolation family** used throughout the Mudfish design
specification (§8 — Sheer Formula, keel rocker). The mapping to Mudfish notation is:

| PDF notation | Mudfish notation | Meaning |
|-------------|-----------------|---------|
| $x$ | $X$ | Longitudinal station position |
| $L_{fore}$ | $BowX$ | Forward half of LWL |
| $L_{aft}$ | $SternX$ | Aft half of LWL |
| $H_{fore}$ | $KR_{Bow}$ | Keel rise at bow (rocker height) |
| $H_{aft}$ | $KR_{Stern}$ | Keel rise at stern |
| $k_{fore}$ | $C_{rocker,fwd}$ | Forward curvature coefficient |
| $k_{aft}$ | $C_{rocker,aft}$ | Aft curvature coefficient |

The keel $Z$ coordinate at any station $X$ in Mudfish coordinates:

$$Z_{keel}(X) = -\text{Draft} + k_{fore} \cdot X^2 \qquad [X \geq 0, \text{ forward}]$$

$$Z_{keel}(X) = -\text{Draft} + k_{aft} \cdot X^2 \qquad [X \leq 0, \text{ aft}]$$

At midship ($X = 0$): $Z_{keel} = -\text{Draft}$ (maximum depth). At the bow tip: $Z_{keel} = -\text{Draft} + H_{fore}$ (keel rises by the rocker amount). The slope is exactly zero at midship — $C^1$ smooth, no kink — matching the lofting tip above.

---

*Source: Boat Hull Keel Rocker Lofting — Parabolic Curve Formula (translated to LaTeX-friendly Markdown for the Mudfish Project, September 15, 2026)*
