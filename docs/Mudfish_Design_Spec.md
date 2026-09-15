# MUDFISH PROJECT
## Matrix Lofting Engine — Current Design Specification

**Date:** September 15, 2026  
**Status:** Living Document — append, never overwrite  
**Repo:** [github.com/sparked-matter/Mudfish](https://github.com/sparked-matter/Mudfish)

---

## §1 Purpose and Governing Philosophy

This document is the single source of truth for the Mudfish Matrix Lofting Engine. It captures every design decision made to date and serves as the contract that all future work — spreadsheet additions, matrix cards, formula cards, continuity tests — must honor without exception.

**Two rules govern this document:**

1. **Append, never overwrite.** New decisions are added as dated sections below existing ones. Nothing already locked is changed without an explicit, dated amendment.
2. **The uploaded workbook is the reference, not any AI's recollection of it.** When in doubt, read the file.

**Emil's two global design principles (apply everywhere):**

1. **Never reinvent what already exists.** Use established standards; don't build custom infrastructure when a mature tool exists.
2. **Never paint yourself into a corner.** Every architectural choice must remain extensible without major trauma.

---

## §2 Locked Coordinate Convention

> **Status: FROZEN. No change permitted without explicit amendment.**

| Axis | Direction |
|------|-----------|
| $+X$ | Forward (bow direction, positive) |
| $-X$ | Aft (stern direction, negative) |
| $+Y$ | Port — left side looking forward *(right-hand rule requires this)* |
| $-Y$ | Starboard |
| $+Z$ | Up (toward sky) |
| $-Z$ | Down (toward keel) |
| $X = 0$ | Midship — the longitudinal reference |
| $Z = Z_{DWL}$ | Design Waterline — the vertical reference ($Z_{DWL} = 0$ in local design frame) |

**Row-vector kernel contract:**

$$P' = P \times M$$

Translation lives in the **last row** of every $4 \times 4$ matrix. This is the transpose of the standard graphics-textbook column-vector convention. The right-hand rule confirms $+Y = \text{port}$ because:

$$\hat{X} \times \hat{Y} = \hat{Z} \quad \Rightarrow \quad \text{forward} \times \text{port} = \text{up} \checkmark$$

---

## §3 The Four-Quadrant Mantra

The hull is divided by two fundamental references — Midship ($X=0$) and the Design Waterline ($Z=Z_{DWL}$) — into four geometric quadrants:

$$\begin{array}{c|cc}
 & \text{Forward} & \text{Aft} \\
\hline
\text{Upper} & F\text{-}U & A\text{-}U \\
\text{Lower} & F\text{-}L & A\text{-}L \\
\end{array}$$

- **F-U** Forward-Upper: $X > 0,\; Z > Z_{DWL}$ — topsides forward of midship
- **A-U** Aft-Upper: $X < 0,\; Z > Z_{DWL}$ — topsides aft of midship
- **F-L** Forward-Lower: $X > 0,\; Z < Z_{DWL}$ — underbody forward of midship
- **A-L** Aft-Lower: $X < 0,\; Z < Z_{DWL}$ — underbody aft of midship

Every formula card and every matrix card is assigned to exactly one quadrant. No formula crosses a quadrant boundary.

The DWL boundary is simultaneously the bottom of the Upper cards and the top of the Lower cards. In the current Mudfish design, the DWL coincides with the chine point **by construction** — a deliberate design choice, not a mathematical requirement. A second chine below DWL adds one more card to the Lower stack, nothing else.

---

## §4 The Golden Layout

The canonical spreadsheet layout is `four_quadrant_mantra.xlsx`, established September 15, 2026.

**All future additions must fit into this layout — not replace it.**

- Forward ($X>0$) and Aft ($X<0$) quadrants sit **side by side horizontally**
- Upper ($Z>Z_{DWL}$) and Lower ($Z<Z_{DWL}$) halves **stack vertically**, separated by a visible DWL divider row
- Each quadrant contains an independent column of matrix cards reading **top-down**: Sheer → DWL → Chine → Keel
- The DWL card is simultaneously the bottom card of the Upper stack and the top reference of the Lower stack

> **Governing rule: incremental refinement only. Never redesign the layout from scratch.**

---

## §5 Card Architecture

### 5.1 Two-Layer Separation

Every card has two independent layers that **never mix**:

| Layer | Location | Content | Produces |
|-------|----------|---------|---------|
| **Formula Layer** | Left side of card | Quadratic formulas, hull parameters, named cells | One clean point $(X, Y, Z, 1)$ |
| **Operator Layer** | Right side of card | $4 \times 4$ homogeneous matrix $M$ | $P' = P \times M$ |

These layers communicate through exactly one handshake: a clean homogeneous row vector $[X,\; Y,\; Z,\; 1]$.

Adding a new formula **never touches a matrix**. Adding a new operator **never touches a formula**. No spaghetti.

### 5.2 Card Types

**ANCHOR** — Sets an absolute position directly. Input $P$ is the actual global point. Matrix $M = I$ (identity, no transformation). Used for the first card in each quadrant stack (currently: SHEER).

**CHAIN** — Rotates a local direction vector and translates it to the previous card's endpoint. Input $P$ is local:

$$P_{local} = [0,\; L\cos\theta,\; L\sin\theta,\; 1]$$

Translation row of $M$ = previous card's output point. The $4 \times 4$ matrix for a CHAIN card is:

$$M = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & \cos\theta & \sin\theta & 0 \\ 0 & -\sin\theta & \cos\theta & 0 \\ X_{prev} & Y_{prev} & Z_{prev} & 1 \end{bmatrix}$$

### 5.3 Current Card Stack (Mudfish)

| Card | Type | $\theta$ (deg) | $L$ (mm) | Global endpoint |
|------|------|---------------|---------|----------------|
| SHEER | ANCHOR | — | — | $(X,\; 1269.13,\; 1200.0)$ |
| DWL | CHAIN | $-96.0°$ | $1206.61$ | $(X,\; 1143.0,\; 0.0)$ |
| CHINE | CHAIN | $-131.52°$ | $1222.0$ | $(X,\; 333.0,\; -915.0)$ |
| KEEL | CHAIN | $180.0°$ | $333.03$ | $(X,\; 0.0,\; -915.0)$ |

Both $\theta$ and $L$ are quadratic laws in $X$:

$$p(X) = a + bX + cX^2$$

Set $b = c = 0$ for a constant (current design). The formula layer evaluates these laws; the operator layer never sees them.

### 5.4 Modularity Rule

To add a chine: copy one CHAIN block → paste below the existing chine card → set its angle and length laws. Nothing else changes. The chain resolves by CardID lookup (INDEX/MATCH on the CardID column), not by row position — copy-paste cannot silently re-wire the stack.

---

## §6 Matrix Kernel — Verified Invariants

> *Status: VERIFIED September 14, 2026 by KernelTests sheet + independent hand calculation + independent ChatGPT inspection.*

Tolerance policy:

$$\varepsilon_{C_0} = \varepsilon_{C_1} = 0.001 \text{ mm}$$

(explicit named cells `eps_C0`, `eps_C1` in the KernelTests sheet — not hardcoded literals)

| Test | Input | Operation | Expected Output | Status |
|------|-------|-----------|----------------|--------|
| TEST-001 | $[1,0,0,1]$ | $\times\; I$ | $[1,0,0,1]$ | **PASS** |
| TEST-002 | $[1,0,0,1]$ | $\times\; R_z(90°)$ | $[0,1,0,1]$ | **PASS** |
| TEST-003 | $[1,2,3,1]$ | $\times\; T(10,20,30)$ | $[11,22,33,1]$ | **PASS** |
| TEST-004 | $[5,7,3,1]$ | $\times\; M_Y$ (mirror) | $[5,-7,3,1]$ | **PASS** |
| TEST-005 | $[1,0,0,1]$ | $P \times R_z \times T \neq P \times T \times R_z$ | Confirmed noncommutative | **PASS** |

TEST-002 confirms the $+Y = \text{port}$ convention: a $+90°$ rotation about $+Z$ maps $\hat{X}$ (forward) to $\hat{Y}$ (port), as the right-hand rule requires.

---

## §7 Hull Continuity — Verified

> *Status: VERIFIED September 14–15, 2026 by HullContinuityFixture v2.*

The four-quadrant surface patches use a **shared-skeleton algebraic family** that guarantees exact boundary continuity by construction:

$$F(X, Z) = B_0 + pX + q\zeta + AX^2 + B\zeta^2 + CX^2\zeta^2$$

where $\zeta = Z - Z_{DWL}$.

Shared terms ($p$, $B_0$) force exact $C^0$ and $C^1$ continuity at midship and DWL boundaries. Independent terms ($A$, $C$) allow each quadrant to differ away from the boundaries.

| Condition | $C^0$ | $C^1$ | Status |
|-----------|-------|-------|--------|
| Smooth midship seam (F-U / A-U) | PASS | PASS | Expected smooth |
| Smooth midship seam (F-L / A-L) | PASS | PASS | Expected smooth |
| DWL seam — forward | PASS | Intentional break | Expected chine |
| DWL seam — aft | PASS | Intentional break | Expected chine |
| Corner invariant $(X=0, Z=Z_{DWL})$ | All four patches = $B_0$ | — | PASS |
| Neg. Control 1: gap $F_{FL} + \delta$ | FAIL (detected) | — | Harness works |
| Neg. Control 2: kink $F_{AU} + \gamma X$ | PASS | FAIL (detected) | Harness works |

> **Key principle:** three sampled PASS rows prove global continuity only when the algebra guarantees it everywhere. When real spline patches replace the skeleton family, dense or adaptive sampling must be added as a backstop.

---

## §8 Sheer Formula — Specified, Not Yet Built

> *Specified: September 15, 2026. Status: EARMARKED — to be wired into the golden layout.*

The sheer line is the intersection of the XY plan view (half-breadth taper) and the XZ profile view (sheer rise curve). Both are quadratic formulas in $X$, same family as the bow taper and keel rocker.

### 8.1 Named Parameters

| Parameter | Description |
|-----------|-------------|
| $FB_{Min}$ | Minimum freeboard height (mm) — the actual lowest point of the sheer |
| $X_{MinFB}$ | Longitudinal position of $FB_{Min}$ (mm). $0 =$ midship. Positive = toward bow. Negative = toward stern. |
| $FB_{Bow}$ | Freeboard at bow tip (mm) — highest point |
| $FB_{Stern}$ | Freeboard at stern (mm) — typically between $FB_{Min}$ and $FB_{Bow}$ |

What was previously called `FreeBoardMax` in the spreadsheet is actually $FB_{Bow}$ — it only coincided with the maximum when the minimum was assumed to be at midship.

### 8.2 Derived Coefficients

$$C_{sheer,fwd} = \frac{FB_{Bow} - FB_{Min}}{(BowX - X_{MinFB})^2}$$

$$C_{sheer,aft} = \frac{FB_{Stern} - FB_{Min}}{(SternX - X_{MinFB})^2}$$

### 8.3 Sheer Z Formula

$$Z_{sheer,fwd}(X) = FB_{Min} + C_{sheer,fwd} \cdot (X - X_{MinFB})^2 \qquad [X \geq X_{MinFB}]$$

$$Z_{sheer,aft}(X) = FB_{Min} + C_{sheer,aft} \cdot (X - X_{MinFB})^2 \qquad [X \leq X_{MinFB}]$$

Setting $X_{MinFB} = 0$ recovers the symmetric midship-minimum case. Setting $FB_{Min} = FB_{Bow} = FB_{Stern}$ recovers a flat deck. No branching, no IF statements — one formula family covers all cases.

### 8.4 Reference Length

The rise percentage is referenced to **LWL (half)**, not LOA and not freeboard:

$$SheerRise_{Bow} = BowRisePct \times BowX \qquad (BowX = \text{forward half of LWL})$$
$$SheerRise_{Stern} = SternRisePct \times SternX \qquad (SternX = \text{aft half of LWL})$$

**Rationale:** LWL governs the hydrodynamic environment. LOA includes overhangs beyond the waterplane. Indexing to LWL keeps the sheer rise proportional to what the boat actually sees in the water.

---

## §9 View Operators vs Section Operators — Distinction Locked

Two fundamentally different operations must never be conflated:

**View Operator (orthographic projection):** Selects components from an already-transformed point for display. Implementation: simple coordinate selection, no matrix multiply needed for pure axis-aligned views.

$$P_{XY} = (X', Y') \qquad P_{XZ} = (X', Z') \qquad P_{YZ} = (Y', Z')$$

**Section/Intersection Operator:** Finds where the hull surface intersects an oblique plane. Requires root-solving or curve extraction. A projection matrix alone does not find an intersection curve.

Oblique views (diagonal plane cuts at arbitrary angles) are **future work** and require explicit root-finding, not just coordinate selection.

---

## §10 Open Items

*As of September 15, 2026:*

- [ ] Wire sheer formula (§8) into the golden layout as live formula cards
- [ ] Add sheer rise visualization in the matrix-visualization style (P × M₁ × M₂ = P')
- [ ] Ballast and stability calculation — flagged as first priority from project inception, still unaddressed
- [ ] Keel rocker formula — same quadratic family as sheer, LWL-referenced, specify and wire in
- [ ] Confirm HullContinuityFixture sampling strategy once real hull equations replace skeleton family
- [ ] FreeCAD Ship workbench shell→solid gap — candidate for open-source contribution
- [ ] Five confirmed FreeCAD UI bugs — documented with GitHub issue numbers, candidate contributions

---

## Amendment Log

| Date | Section | Description |
|------|---------|-------------|
| 2026-09-15 | All | Initial specification created. §1–§9 established. |

*Future amendments appended here with date and section reference.*

---

*MUDFISH is open-source under LGPL-3.0. Contributions welcome at [github.com/sparked-matter/Mudfish](https://github.com/sparked-matter/Mudfish)*
