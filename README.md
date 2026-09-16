# Mudfish

**Half sailboat, half survival pod — built with surviving harsh conditions in its DNA.**

**An open-source, LGPL-licensed, fully parametric aluminum sailboat, designed for ocean crossing.**

> **World first:** Mudfish is believed to be the world's first sailboat design released under a formal open-source license (LGPL-3.0) with a fully parametric computational model, version-controlled documentation, and a public repository. Prior "free plan" projects (such as the YAGO 31, Germany, 2004) explicitly described themselves as only *"analog to open source"* — they lacked a legal open-source framework, parametric geometry, or version control. Mudfish is the complete realization of that idea, not an approximation of it. This repository was made public on GitHub in 2026. All commits are cryptographically timestamped and permanently recorded. If you know of an earlier project that meets all criteria — LGPL license, parametric model, version-controlled, public repository — please open a Discussion. We want the record to be accurate.


Twin bilge keels for beachability and easy transport — no centerboard, no centerboard trunk, dries out upright on any beach or mudflat. Riveted aluminum 5086 construction, buildable with hand tools and patience — no welder required. A hull that bounces off hard objects instead of breaking, and can be repaired in the field by drilling out and replacing panels. Fully parametric design — change one parameter and the entire hull, weight budget, and structural analysis update everywhere. Full CNC-ready build files with pre-punched rivet holes for self-aligning assembly, released free for anyone to build, study, modify, and improve.

Mudfish is a flagship project of [Sparked Matter LLC](https://sparkedmatter.com).

---

## Why Mudfish

Most small-boat design plans are sold, closed, and undocumented — you get a set of drawings and little else. Mudfish takes a different approach:

- **Open.** Full design files, calculations, and reasoning are public, under LGPL-3.0.
- **Accessible.** Designed around amateur-buildable aluminum stitch-and-weld construction, not a professional boatyard.
- **Transparent.** Every major design decision is documented in the [Engineering Log](./engineering-log) — the formulas, the tradeoffs, and yes, the AI-assisted exploration that helped get there. Nothing is a black box.

## Status

🚧 **Early development.** Hull form and stability calculations are the current priority — see the [Engineering Log](./engineering-log) for active work. No finalized build files yet. Watch/star the repo to follow progress.

## Design Philosophy

- **Twin bilge keels** instead of a fin keel or centerboard — dries out upright on a beach or mudflat, simplifies construction, no moving parts below the waterline.
- **Aluminum, not fiberglass or plywood** — dents instead of cracking on impact with reefs, containers, or debris. Variable plate thickness by zone (heavier at the keel/bottom, lighter above the waterline) balances toughness against weight.
- **Aluminum 5086 alloy** — marine-grade, weldable, excellent corrosion resistance in saltwater. The leading construction approach under consideration is **solid riveted panels with Sikaflex sealant bedding** — no welder required, CNC-punched rivet holes provide self-alignment, field repair is straightforward, and the mechanical fastening reduces stress concentration compared to welded seams. MIG welding remains under evaluation as an alternative path.
- **Rivet-first philosophy** — solid rivets + Sikaflex is accessible to any builder with hand tools and patience. Pre-punched CNC panels self-align during assembly, eliminating the need for complex jigs. Damaged panels can be drilled out and replaced in the field. The cool factor of a riveted aluminum hull — echoing aircraft and classic step-van construction — is a feature, not a compromise.
- **Composite hybrid construction** — riveting is inherently material-agnostic in a way welding never can be. You cannot weld aluminum to oak; you can absolutely rivet aluminum to oak, G10, or fiberglass. This opens the door to a true hybrid structure: aluminum topsides and keel panels riveted to a hardwood or G10 stem (bow takes the most impact — make it the toughest material), a longitudinal keel backbone in oak or G10 for rigidity, and a G10 or fiberglass transom (better for through-hulls and outboard mounting). Each structural member carries its own material spec in the parametric model — the hull geometry doesn't change, only the material assigned to each member.
- **AI-accelerated, human-verified** — FreeCAD and modern engineering tools shorten the iteration cycle dramatically. Every calculation is still checked against real naval architecture principles, not just AI output.
- **Material-agnostic parametric core** — the reference design targets aluminum 5086, but the parametric architecture is designed to accommodate alternative materials: marine plywood, cold-molded wood, fiberglass/foam composite, basalt fiber composite, or other structural materials. Different materials require different plate thickness laws and structural calculations, but the hull geometry engine is not locked to any one material.

## Repository Structure

```
mudfish/
├── README.md              this file
├── LICENSE                LGPL-3.0 full text
├── CONTRIBUTING.md        how to propose changes, ask questions, get involved
├── engineering-log/       dated record of design decisions — the "why," not just the "what"
├── cad/                   FreeCAD source files (coming as work progresses)
└── cutfiles/              CNC-ready DXF/nested layouts (coming once hull form is finalized)
```

## The Digital Advantage

Mudfish is not just a set of boat plans. It is a fully parametric, fully digital design system — and that distinction matters.

Because the hull geometry is driven by named parameters (beam, draft, freeboard, rocker, flare angle, material thickness), every structural member is mathematically defined rather than hand-drawn. Change the plate gauge from 4mm to 3mm and the weight budget updates everywhere. Swap the stem material from aluminum to G10 and the joint analysis updates. Add a second chine and the entire hull geometry recomputes.

This parametric foundation enables a class of analysis that closed, static boat plans simply cannot offer:

- **FEA (Finite Element Analysis)** — structural stress simulation under wave loading, impact, and rig tension, run against the actual hull geometry with real material properties
- **Hydrostatics and stability** — displacement, center of buoyancy, righting moment curves, all computed from the live parametric model via FreeCAD's Ship workbench
- **Weight and balance** — full weight budget from material density × volume for every structural member, automatically updated when materials or dimensions change
- **Longitudinal strength** — bending moment and shear force along the hull length under realistic load cases
- **Material comparison** — run the same hull in 5086 aluminum, marine plywood, basalt fiber composite, or G10 and compare weight, cost, and structural performance side by side

All of this is open, documented, and baked into the design files. Builders who want to verify, modify, or extend any calculation have everything they need. Nothing is a black box. Nothing requires purchasing a proprietary software license. The entire digital thread — from hull parameter to CNC cut file — is free, open, and reproducible.

---

## Getting Started

This project is early — there isn't yet a build-ready file set. The best way to follow along right now:

1. **Star/watch this repo** for updates.
2. **Read the [Engineering Log](./engineering-log)** to see current design thinking and open questions.
3. **Open a [Discussion](../../discussions)** if you have naval architecture, welding, or CNC experience and want to weigh in on an open question.

## License

Mudfish is released under the **GNU Lesser General Public License v3.0** — see [LICENSE](./LICENSE). You are free to build, study, modify, and share this design, including forking it in your own direction.

## About Sparked Matter

Sparked Matter LLC works at the intersection of three things: **it matters** (real problems, real purpose), **subject matter** (deep engineering knowledge), and **physical matter** (ideas made into working hardware). Mudfish is that philosophy applied to a hull. Learn more at [sparkedmatter.com](https://sparkedmatter.com).

